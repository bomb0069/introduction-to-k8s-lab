# Headless Service

## Start Headless

```shell
kubectl apply -f 01-headless.yaml
```

```shell
$ kubectl get pod --watch
            
NAME                                 READY   STATUS            RESTARTS   AGE
web-0                                0/1     PodInitializing   0          5s
client-deployment-54854fbc58-t9knf   1/1     Running           0          5s
web-0                                1/1     Running           0          6s
web-1                                0/1     Pending           0          0s
web-1                                0/1     Pending           0          0s
web-1                                0/1     Init:0/1          0          0s
web-1                                0/1     PodInitializing   0          3s
web-1                                1/1     Running           0          6s
web-2                                0/1     Pending           0          0s
web-2                                0/1     Pending           0          0s
web-2                                0/1     Init:0/1          0          0s
web-2                                0/1     PodInitializing   0          4s
web-2                                1/1     Running           0          7s
```

wait until it all running (Break with `Ctrl + C`)

## Run Test

### Check Service Information

```shell
$ kubectl get service -o wide

NAME                      TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE   SELECTOR
kubernetes                ClusterIP   10.43.0.1    <none>        443/TCP   21m   <none>
my-web-service            ClusterIP   10.43.91.7   <none>        80/TCP    71s   app=nginx
my-web-service-headless   ClusterIP   None         <none>        80/TCP    71s   app=nginx
```

```shell
$ kubectl describe service my-web-service-headless 

Name:              my-web-service-headless
Namespace:         default
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                None
IPs:               None
Port:              web  80/TCP
TargetPort:        80/TCP
Endpoints:         10.42.1.6:80,10.42.2.6:80,10.42.3.9:80
Session Affinity:  None
Events:            <none>

$ kubectl describe service my-web-service    

Name:              my-web-service
Namespace:         default
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.43.91.7
IPs:               10.43.91.7
Port:              web  80/TCP
TargetPort:        80/TCP
Endpoints:         10.42.1.6:80,10.42.2.6:80,10.42.3.9:80
Session Affinity:  None
Events:            <none>
```

### Run Curl with Service

```shell
$ kubectl get pod

NAME                                 READY   STATUS    RESTARTS   AGE
client-deployment-54854fbc58-t9knf   1/1     Running   0          5m23s
web-0                                1/1     Running   0          5m23s
web-1                                1/1     Running   0          5m7s
web-2                                1/1     Running   0          4m42s
```

```shell
$ kubectl exec -it ${POD_NAME} -- curl my-web-service

0
```

```shell
$ kubectl exec -it ${POD_NAME} -- curl my-web-service

1
```

```shell
$ kubectl exec -it ${POD_NAME} -- curl my-web-service

2
```

### Run Curl with Service

```shell
kubectl exec -it ${POD_NAME} -- curl my-web-service-headless

1
```

```shell
kubectl exec -it ${POD_NAME} -- curl my-web-service-headless

1
```

```shell
kubectl exec -it ${POD_NAME} -- curl my-web-service-headless

1
```

## Clear

```shell
kubectl delete -f 01-headless.yaml
```