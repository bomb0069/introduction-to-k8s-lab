# Manage Namespace

## List

```shell
$ kubectl get namespace 

NAME              STATUS   AGE
kube-system       Active   123m
default           Active   123m
kube-public       Active   123m
kube-node-lease   Active   123m
```

## Create

### Create with Command

```shell
$ kubectl create namespace manual-namespace

namespace/manual-namespace created
```

### Create with Configuration File

```shell
$ kubectl apply -f my-namespace.yaml

namespace/my-namespace created
```

## Delete Namepace

```shell
$ kubectl delete namespace manual-namespace

namespace "manual-namespace" deleted
```

```shell
$ kubectl delete -f my-namespace.yaml

amespace "my-namespace" deleted
```

can't delete `default` namespaces

```shell
$ kubectl delete namespace default
Error from server (Forbidden): namespaces "default" is forbidden: this namespace may not be deleted
```
