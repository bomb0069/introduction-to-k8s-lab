# Command kubectl wth Namespace

## Setup

create 2 difference namespaces

```shell
$ kubectl apply -f diff-2-environment.yaml

namespace/dev-environment created
namespace/uat-environment created
```

## Apply resource into dev-environment

```shell
$ kubectl apply -f simple-app.1.0.yaml --namespace=dev-environment
```

```shell
$ kubectl get all --namespace=dev-environment

NAME                                    READY   STATUS    RESTARTS   AGE
pod/my-web-deployment-85454d7c4-xd7c4   1/1     Running   0          29s
pod/my-web-deployment-85454d7c4-px8vg   1/1     Running   0          29s

NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/my-web-service   ClusterIP   10.43.252.39   <none>        80/TCP    29s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-web-deployment   2/2     2            2           29s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/my-web-deployment-85454d7c4   2         2         2       29s
```

## check resource in uat-environment

```shell
$ kubectl get all --namespace=uat-environment
```

## Apply resource into uat-environment

```shell
$ kubectl apply -f simple-app.1.0.yaml --namespace=uat-environment
```

```shell
$ kubectl get all --namespace=uat-environment 

NAME                                    READY   STATUS              RESTARTS   AGE
pod/my-web-deployment-85454d7c4-8lrc9   0/1     ContainerCreating   0          6s
pod/my-web-deployment-85454d7c4-ndkwh   1/1     Running             0          6s

NAME                     TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/my-web-service   ClusterIP   10.43.131.6   <none>        80/TCP    6s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-web-deployment   1/2     2            1           6s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/my-web-deployment-85454d7c4   2         2         1       6s
```

## apply ingress to 2 environment

```shell
kubectl apply -f ingress.yaml

ingress.networking.k8s.io/my-web-ingress created
ingress.networking.k8s.io/my-web-ingress created
```

## config host file for seperate Domain

```shell
sudo vi /etc/hosts
```

```hosts
##
# Host Database
#

127.0.0.1    dev.my-env.com
127.0.0.1    uat.my-env.com
```

## Try to open browser

http://dev.my-env.com and http://uat.my-env.com

## Upgrade Dev to version 2.0

```shell
$ kubectl apply -f simple-app.2.0.yaml --namespace=dev-environment

deployment.apps/my-web-deployment configured
service/my-web-service unchanged
```

```shell
kubectl get pod --watch  --namespace=dev-environment

NAME                                READY   STATUS    RESTARTS   AGE
my-web-deployment-85454d7c4-mkx2p   1/1     Running   0          41s
my-web-deployment-85454d7c4-trrp9   1/1     Running   0          37s
```

## Try to open browser

http://dev.my-env.com and http://uat.my-env.com

## Clear

delete workshop's namespace

```shell
$ kubectl delete -f kubectl delete -f diff-2-environment.yaml
 
namespace "dev-environment" deleted
namespace "uat-environment" deleted
```