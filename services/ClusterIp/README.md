# Cluster IP Service

## Lab 1 Simple Cluster IP

### Start

```shell
kubectl apply -f 01-simple.yaml
```

### Run Test

```shell
$ kubectl get pod --watch

NAME                                READY   STATUS              RESTARTS   AGE
my-web-deployment-b6876bb6b-5w99h   0/1     ContainerCreating   0          2s
my-web-deployment-b6876bb6b-jsvz9   0/1     ContainerCreating   0          2s
my-web-deployment-b6876bb6b-jsvz9   1/1     Running             0          4s
my-web-deployment-b6876bb6b-5w99h   1/1     Running             0          4s
```

wait until it all running (Break with `Ctrl + C`)

open browser with http://localhost:8888

### Get Service

```shell
$ kubectl get service

NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes       ClusterIP   10.43.0.1      <none>        443/TCP   2m37s
my-web-service   ClusterIP   10.43.25.152   <none>        80/TCP    46s
```

```shell
$ kubectl get service -o wide

NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE    SELECTOR
kubernetes       ClusterIP   10.43.0.1      <none>        443/TCP   3m2s   <none>
my-web-service   ClusterIP   10.43.25.152   <none>        80/TCP    71s    app=my-web
```

### Clear

```shell
kubectl delete -f 01-simple.yaml
```

## Lab 2 Multiple Deployment


### Start

```shell
kubectl apply -f 02-multi-deployment.yaml
```

### Run Test

```shell
$ kubectl get pod --watch
            
NAME                                   READY   STATUS              RESTARTS   AGE
my-https-deployment-77b87fcb7d-spvv5   0/1     ContainerCreating   0          1s
my-nginx-deployment-b6876bb6b-rczl6    0/1     ContainerCreating   0          1s
my-nginx-deployment-b6876bb6b-rczl6    1/1     Running             0          4s
my-https-deployment-77b87fcb7d-spvv5   1/1     Running             0          4s
```

wait until it all running (Break with `Ctrl + C`)

open browser with http://localhost:8888 and try to refesh

### Clear

```shell
kubectl delete -f 02-multi-deployment.yaml
```
