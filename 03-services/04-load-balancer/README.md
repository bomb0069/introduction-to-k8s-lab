# Load Balancer

## Start

### Run

```shell
kubectl apply -f 01-simple-loadbalancer.yaml 
```

```shell
$ kubectl get pod --watch     
NAME                                READY   STATUS              RESTARTS   AGE
my-web-deployment-b6876bb6b-ntfcr   0/1     ContainerCreating   0          5s
my-web-deployment-b6876bb6b-rn6g9   0/1     ContainerCreating   0          5s
svclb-my-web-service-zcqn2          0/1     ContainerCreating   0          5s
svclb-my-web-service-kgmcr          0/1     ContainerCreating   0          5s
svclb-my-web-service-tvt8t          0/1     ContainerCreating   0          5s
svclb-my-web-service-scjx9          0/1     ContainerCreating   0          5s
svclb-my-web-service-tvt8t          1/1     Running             0          7s
svclb-my-web-service-zcqn2          1/1     Running             0          8s
svclb-my-web-service-scjx9          1/1     Running             0          9s
svclb-my-web-service-kgmcr          1/1     Running             0          9s
my-web-deployment-b6876bb6b-rn6g9   1/1     Running             0          13s
my-web-deployment-b6876bb6b-ntfcr   1/1     Running             0          14s
```

wait until it all running (Break with `Ctrl + C`)

### Check Service

```shell
$ kubectl get service -o wide

NAME             TYPE           CLUSTER-IP      EXTERNAL-IP                                   PORT(S)        AGE   SELECTOR
kubernetes       ClusterIP      10.43.0.1       <none>                                        443/TCP        89s   <none>
my-web-service   LoadBalancer   10.43.220.227   172.29.0.2,172.29.0.3,172.29.0.4,172.29.0.5   80:30080/TCP   51s   app=my-web
```

```shell
$ kubectl describe service my-web-service

Name:                     my-web-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=my-web
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.43.220.227
IPs:                      10.43.220.227
LoadBalancer Ingress:     172.29.0.2, 172.29.0.3, 172.29.0.4, 172.29.0.5
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30080/TCP
Endpoints:                10.42.0.4:80,10.42.1.4:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

### Check Result

open browser with http://localhost:8888

## Clear

```shell
kubectl delete -f 01-simple-loadbalancer.yaml
```
