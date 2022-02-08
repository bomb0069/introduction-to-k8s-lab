# Configuration File with Namespace

## Setup List

```shell
$ kubectl get namespace 

NAME              STATUS   AGE
kube-system       Active   123m
default           Active   123m
kube-public       Active   123m
kube-node-lease   Active   123m
```

Create Namespace

```shell
$ kubectl apply -f my-namespace.yaml

namespace/my-namespace created
```

## Apply resource into my-namespace

```shell
$ kubectl apply -f simple-app-with-namespace.yaml

deployment.apps/my-web-deployment created
service/my-web-service created
ingress.networking.k8s.io/my-web-ingress created
```

## Clear

```shell
$ kubectl delete -f my-namespace.yaml

namespace "my-namespace" deleted
```