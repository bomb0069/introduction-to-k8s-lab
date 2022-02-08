# Command kubectl wth Namespace

## Setup

create 2 difference namespaces

```shell
$ kubectl apply -f diff-2-namespaces.yaml

namespace/my-dev-namespace created
namespace/my-uat-namespace created
```

## Apply resource into my-dev-namespace

```shell
$ kubectl apply -f 01-simple.yaml --namespace=my-dev-namespace
```

```shell
$ kubectl get all --namespace=my-dev-namespace 

NAME                                    READY   STATUS    RESTARTS   AGE
pod/my-web-deployment-b6876bb6b-jw9p8   1/1     Running   0          9s
pod/my-web-deployment-b6876bb6b-sr58m   1/1     Running   0          9s

NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/my-web-service   ClusterIP   10.43.201.58   <none>        80/TCP    9s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-web-deployment   2/2     2            2           9s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/my-web-deployment-b6876bb6b   2         2         2       9s
```

## check resource in my-uat-namespace

```shell
$ kubectl get all --namespace=my-uat-namespace
```

## Apply resource into my-uat-namespace

```shell
$ kubectl apply -f 01-simple.yaml --namespace=my-uat-namespace
```

```shell
$ kubectl get all --namespace=my-uat-namespace 

NAME                                    READY   STATUS    RESTARTS   AGE
pod/my-web-deployment-b6876bb6b-4j4h7   1/1     Running   0          7s
pod/my-web-deployment-b6876bb6b-m6sqc   1/1     Running   0          7s

NAME                     TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/my-web-service   ClusterIP   10.43.0.231   <none>        80/TCP    7s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-web-deployment   2/2     2            2           7s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/my-web-deployment-b6876bb6b   2         2         2       7s
```

## Clear

delete workshop's namespace

```shell
$ kubectl delete -f diff-2-namespaces.yaml

namespace "my-dev-namespace" deleted
namespace "my-uat-namespace" deleted
```