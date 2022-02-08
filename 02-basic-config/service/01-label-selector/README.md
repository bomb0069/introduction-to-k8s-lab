# Missing Selector

## Run

```shell
$ kubectl apply -f missing.yaml

deployment.apps/my-web-deployment created
```

## Check

list all objects

```shell
$ kubectl get all -o wide      
NAME                                    READY   STATUS              RESTARTS   AGE   IP       NODE                     NOMINATED NODE   READINESS GATES
pod/my-web-deployment-b6876bb6b-96jwz   0/1     ContainerCreating   0          5s    <none>   k3d-my-cluster-agent-1   <none>           <none>
pod/my-web-deployment-b6876bb6b-44k7z   0/1     ContainerCreating   0          5s    <none>   k3d-my-cluster-agent-2   <none>           <none>

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE    SELECTOR
service/kubernetes       ClusterIP   10.43.0.1       <none>        443/TCP   130m   <none>
service/my-web-service   ClusterIP   10.43.215.115   <none>        80/TCP    5s     app=my-web-deployment

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS        IMAGES   SELECTOR
deployment.apps/my-web-deployment   0/2     2            0           5s    my-web-on-nginx   nginx    app=my-web

NAME                                          DESIRED   CURRENT   READY   AGE   CONTAINERS        IMAGES   SELECTOR
replicaset.apps/my-web-deployment-b6876bb6b   2         2         0       5s    my-web-on-nginx   nginx    app=my-web,pod-template-hash=b6876bb6b
```

describe deployment

```shell
$  kubectl describe service my-web-service 
Name:              my-web-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=my-web-deployment
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.215.115
IPs:               10.43.215.115
Port:              <unset>  80/TCP
TargetPort:        80/TCP
Endpoints:         <none>
Session Affinity:  None
Events:            <none>
```

## Clear

```shell
$ kubectl delete -f missing.yaml     

deployment.apps "my-web-deployment" deleted
```

## Run correct selector

```shell
$ kubectl apply -f simple.yaml

deployment.apps/my-web-deployment created
```

## Check

list all objects

```shell
$ kubectl get all -o wide      
NAME                                    READY   STATUS    RESTARTS   AGE   IP           NODE                     NOMINATED NODE   READINESS GATES
pod/my-web-deployment-b6876bb6b-pzxxm   1/1     Running   0          22s   10.42.1.11   k3d-my-cluster-agent-2   <none>           <none>
pod/my-web-deployment-b6876bb6b-9rn24   1/1     Running   0          22s   10.42.3.8    k3d-my-cluster-agent-1   <none>           <none>

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE    SELECTOR
service/kubernetes       ClusterIP   10.43.0.1       <none>        443/TCP   135m   <none>
service/my-web-service   ClusterIP   10.43.144.141   <none>        80/TCP    22s    app=my-web

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS        IMAGES   SELECTOR
deployment.apps/my-web-deployment   2/2     2            2           22s   my-web-on-nginx   nginx    app=my-web

NAME                                          DESIRED   CURRENT   READY   AGE   CONTAINERS        IMAGES   SELECTOR
replicaset.apps/my-web-deployment-b6876bb6b   2         2         2       22s   my-web-on-nginx   nginx    app=my-web,pod-template-hash=b6876bb6b
```

describe deployment

```shell
$ kubectl describe service my-web-service

Name:              my-web-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=my-web
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.144.141
IPs:               10.43.144.141
Port:              <unset>  80/TCP
TargetPort:        80/TCP
Endpoints:         10.42.1.11:80,10.42.3.8:80
Session Affinity:  None
Events:            <none>
```

## Clear

```shell
$ kubectl delete -f simple.yaml     

deployment.apps "my-web-deployment" deleted
```