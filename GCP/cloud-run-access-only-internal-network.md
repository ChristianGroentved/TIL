# Access Cloud Run Service only when connected to the internal network

At the time of writing everything needs to be in the same region.

## Assumptions
- Use a shared VPC
- A subnet for the project has been set

To connect properly you need a **proxy-only subnet** that can be used for all internal load balancers in a given region

```shell
gcloud compute networks subnets create proxy-only-subnet-es-prod \
  --purpose=REGIONAL_MANAGED_PROXY \
  --role=ACTIVE \
  --region=europe-west4 \
  --network=projects/network-host-project-5361/global/networks/network-host-project-high-trusted-zone-shared-vpc \
  --range=10.235.142.0/26 \ # The range needs to be reserved
  --project=network-host-project-5361
```
The proxy won't be assigned anywhere it just needs to be there.

Next, we need to create a Serverless Network Endpoint Group (NEG)
```shell
 gcloud compute network-endpoint-groups create hello-world-serverless-neg \
  --region=europe-west4 \
  --network-endpoint-type=SERVERLESS \
  --cloud-run-service=tagging-service \
  --project=es-prod-6c2a
```

The, create a backend service and connect the NEG to it
```shell
gcloud compute backend-services create hello-world-backend-service \
  --load-balancing-scheme INTERNAL_MANAGED \
  --protocol=HTTPS \
  --region=europe-west4 \
  --project=es-prod-6c2a

gcloud compute backend-services add-backend hello-world-backend-service \
  --network-endpoint-group=hello-world-serverless-neg \
  --network-endpoint-group-region=europe-west4 \
  --project=es-prod-6c2a
```

The Backend Service now represents your Cloud Run service to Cloud Load Balancing

Now we will configure the rest of the Internal HTTP(S) Load Balancer components

```shell
gcloud compute url-maps create hello-world-url-map \
  --default-service hello-world-backend-service \
  --region=europe-west4 \
  --project=es-prod-6c2a

gcloud compute target-http-proxies create hello-world-http-proxy \
  --url-map=hello-world-url-map \
  --region=europe-west4 \
  --project=es-prod-6c2a


gcloud compute forwarding-rules create l7-ilb-forwarding-rule \
  --load-balancing-scheme=INTERNAL_MANAGED \
  --network=projects/network-host-project-5361/global/networks/network-host-project-high-trusted-zone-shared-vpc \
  --subnet=projects/network-host-project-5361/regions/europe-west4/subnetworks/es-prod-1 \
  --address=10.235.129.20 \
  --ports=80 \
  --region=europe-west4 \
  --target-http-proxy=hello-world-http-proxy \
  --target-http-proxy-region=europe-west4 \
  --project=es-prod-6c2a
```

Assuming you spelled everything correctly things should be up and running