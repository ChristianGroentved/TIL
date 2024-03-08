# Connect to VM using Identity Aware Proxy

When in need of accessing a VM which do not have an external IP address or which isn't permitted to have direct access over the internet, you can use IAP.
* Create a firewall rule that applies to all instances you need access to.
* Allow ingress traffic from IP range `35.235.240.0/20` That is the range used by IAP
* Allow connection to all ports you want to be accessible e.g. port `22` for SHH or port `3389` for RDP

When the rule has been created access to your instance depends on which type of connection.

For SHH connection use the following in your terminal
```shell
gcloud compute ssh INSTANCE_NAME --project PROJECT_NAME
```
replace `INSTANCE_NAME` with the name of your instance and `PROJECT_NAME` with your project name

For RDP connection use the following in your terminal
```shell
gcloud compute start-iap-tunnel INSTANCE_NAME 3389 \
    --local-host-port=localhost:LOCAL_PORT \
    --zone=ZONE \
    --project PROJECT_NAME
```
Replace `INSTANCE_NAME`, `LOCAL_PORT`, `ZONE`, and `PROJECT_NAME` with appropriate values After that keep the terminal running, additional information can be found [here](https://cloud.google.com/iap/docs/using-tcp-forwarding)