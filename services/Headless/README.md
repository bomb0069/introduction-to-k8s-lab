# Headless Service

## Start Headless

```shell
kubectl apply -f 01-headless.yaml
```

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