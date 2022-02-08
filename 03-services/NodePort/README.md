# NodePort

## Setup Workshop

1. list K3d cluster in your machine

   ```shell
   $ k3d cluster list

   NAME         SERVERS   AGENTS   LOADBALANCER
   my-cluster   1/1       3/3      true
   ```

2. delete my-cluster

   ```shell
   k3d cluster delete my-cluster
   ```

3. create new cluster for this workshop

   ```
   k3d cluster create my-cluster-nodeport -p "8888:80@loadbalancer:0" -p "30080:30080@agent:0" -p "31080:30080@agent:1" --agents 2
   ```

## Start

### Run

```shell
kubectl apply -f 01-simple-nodeport.yaml 

```

```shell
$ kubectl get pod --watch

NAME                                READY   STATUS              RESTARTS   AGE
my-web-deployment-b6876bb6b-mwn6n   0/1     ContainerCreating   0          4s
my-web-deployment-b6876bb6b-vs9kn   0/1     ContainerCreating   0          4s
my-web-deployment-b6876bb6b-vs9kn   1/1     Running             0          4s
my-web-deployment-b6876bb6b-mwn6n   1/1     Running             0          4s
```

wait until it all running (Break with `Ctrl + C`)

### Check Service

```shell
$ kubectl get service -o wide

NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE   SELECTOR
kubernetes       ClusterIP   10.43.0.1       <none>        443/TCP        53s   <none>
my-web-service   NodePort    10.43.212.126   <none>        80:30080/TCP   22s   app=my-web
```

### Check Result

open browser with http://localhost:8888
open browser with http://localhost:30080
open browser with http://localhost:31080

## Clear

```shell
kubectl delete -f 01-simple-nodeport.yaml
```

```shell
k3d cluster delete my-cluster-nodeport
```

Create my-cluster back

```shell
k3d cluster create my-cluster --servers 1 --agents 3 --port "8888:80@loadbalancer" --port "8889:443@loadbalancer"
```
