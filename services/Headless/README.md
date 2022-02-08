# Headless Service

## Start Headless

```shell
kubectl apply -f 01-headless.yaml
```

```shell
$ kubectl get pod --watch
            
NAME                                 READY   STATUS            RESTARTS   AGE
web-0                                0/1     PodInitializing   0          5s
client-deployment-54854fbc58-9s2pk   1/1     Running           0          5s
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

### Run Curl with Service

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