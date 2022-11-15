# Force delete namespace

Suppose you have a namespace stuck in a terminating state. Which could be hindring your destruction of a k8s cluster. You can force delete the namespace in the following way.
First check the state of your namespace. To ensure that it is actually stuck in a terminating state.

```shell
kubectl get ns <ROGUE-NAMESPACE>
```

Running the following command will force remove the namespace. Be aware this could leave ressources dangling only do this if you are sure you do not need the namespace anymore.

```shell
NAMESPACE=your-rogue-namespace
kubectl proxy &
kubectl get namespace $NAMESPACE -o json |jq '.spec = {"finalizers":[]}' >temp.json
curl -k -H "Content-Type: application/json" -X PUT --data-binary @temp.json 127.0.0.1:8001/api/v1/namespaces/$NAMESPACE/finalize
```

This is based of [this](https://stackoverflow.com/questions/52369247/namespace-stuck-as-terminating-how-i-removed-it) answer on stackoverflow
