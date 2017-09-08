# setup

Startup a 3 node Kuberentes cluster.

Run this in a shell to see the whats happening in the cluster:
```
watch -t -n1 'echo ROOK PODS && kubectl get pods -n rook -o wide && echo && echo PODS && kubectl get pods -o wide && echo && echo PVC && kubectl get pvc && echo && echo SVC && kubectl get svc --namespace=rook && echo && kubectl get svc && echo && kubectl get nodes -o wide'
```

# demo

First create the operator:

```
kubectl create -f operator.yaml
```

Next create the cluster:

```
kubectl create -f cluster.yaml
```

Next create the pool:

```
kubectl create -f pool.yaml
```

Run the wordpress app:
```
kubectl create -f wordpress.yaml
```

Kill mysql underneath wordpress:
```
kubectl delete pod [mysql-pod-name]
```

# cleanup

to cleanup run the following:

```
kubectl delete -f wordpress.yaml

kubectl delete -f pool.yaml
kubectl delete -n rook cluster rook
kubectl delete -n rook serviceaccount rook-api
kubectl delete clusterrole rook-api
kubectl delete clusterrolebinding rook-api
kubectl delete thirdpartyresources cluster.rook.io pool.rook.io  # ignore errors if on K8s 1.7+
kubectl delete crd clusters.rook.io pools.rook.io  # ignore errors if on K8s 1.5 and 1.6
kubectl delete secret rook-rook-user

kubectl delete namespace rook

kubectl delete -f operator.yaml
```
