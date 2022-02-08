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

## Get Resource in namespace

normal get pod command

```shell
$ kubectl get pod 

No resources found in default namespace.
```

normal get pod with specific namespace

```shell
$ kubectl get pod --namespace=my-namespace

No resources found in my-namespace namespace.
```

```shell
$ kubectl get pod -n my-namespace

No resources found in my-namespace namespace.
```

## Get Resource from all namespace

```shell
$ kubectl get pod --all-namespaces

NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   local-path-provisioner-84bb864455-d68f6   1/1     Running     0          7m3s
kube-system   coredns-96cc4f57d-6vqmp                   1/1     Running     0          7m3s
kube-system   helm-install-traefik-crd--1-xtpxp         0/1     Completed   0          7m3s
kube-system   metrics-server-ff9dbcb6c-5sw9t            1/1     Running     0          7m3s
kube-system   helm-install-traefik--1-fcndr             0/1     Completed   2          7m3s
kube-system   svclb-traefik-xtjxg                       2/2     Running     0          6m25s
kube-system   svclb-traefik-g6pl2                       2/2     Running     0          6m25s
kube-system   svclb-traefik-jzwp4                       2/2     Running     0          6m25s
kube-system   svclb-traefik-bbfjv                       2/2     Running     0          6m25s
kube-system   traefik-55fdc6d984-wfktl                  1/1     Running     0          6m25s
```

```shell
$ kubectl get pod -A

NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   local-path-provisioner-84bb864455-d68f6   1/1     Running     0          7m3s
kube-system   coredns-96cc4f57d-6vqmp                   1/1     Running     0          7m3s
kube-system   helm-install-traefik-crd--1-xtpxp         0/1     Completed   0          7m3s
kube-system   metrics-server-ff9dbcb6c-5sw9t            1/1     Running     0          7m3s
kube-system   helm-install-traefik--1-fcndr             0/1     Completed   2          7m3s
kube-system   svclb-traefik-xtjxg                       2/2     Running     0          6m25s
kube-system   svclb-traefik-g6pl2                       2/2     Running     0          6m25s
kube-system   svclb-traefik-jzwp4                       2/2     Running     0          6m25s
kube-system   svclb-traefik-bbfjv                       2/2     Running     0          6m25s
kube-system   traefik-55fdc6d984-wfktl                  1/1     Running     0          6m25s
```

## Setting the namespace preference

```shell
$ kubectl config set-context --current --namespace=manual-namespace

Context "k3d-my-cluster" modified.

$ kubectl get pod                                                  
No resources found in manual-namespace namespace.

```

`Important :` switch back to `default` namespace

```shell
$ kubectl config set-context --current --namespace=default

Context "k3d-my-cluster" modified.

$ kubectl get pod

No resources found in default namespace.
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
