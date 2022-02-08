# Mininum Required for Deployment

## Run

```shell
$ kubectl apply -f replica.yaml

deployment.apps/nginx-deployment configured
```

## Check

list all objects

```shell
$ kubectl get all -o wide

NAME                                    READY   STATUS    RESTARTS   AGE   IP          NODE                     NOMINATED NODE   READINESS GATES
pod/nginx-deployment-66b6c48dd5-ppf9f   1/1     Running   0          6s    10.42.1.8   k3d-my-cluster-agent-2   <none>           <none>
pod/nginx-deployment-66b6c48dd5-5ghtm   1/1     Running   0          6s    10.42.3.6   k3d-my-cluster-agent-1   <none>           <none>

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE    SELECTOR
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   118m   <none>

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-deployment   2/2     2            2           6s    nginx        nginx:1.14.2   app=nginx

NAME                                          DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         SELECTOR
replicaset.apps/nginx-deployment-66b6c48dd5   2         2         2       6s    nginx        nginx:1.14.2   app=nginx,pod-template-hash=66b6c48dd5
```

## scale deployment with command

```shell
$ kubectl scale --replicas=5 deployment/nginx-deployment

deployment.apps/nginx-deployment scaled
```

```shell
$ kubectl get all -o wide
NAME                                    READY   STATUS    RESTARTS   AGE     IP          NODE                      NOMINATED NODE   READINESS GATES
pod/nginx-deployment-66b6c48dd5-ppf9f   1/1     Running   0          3m47s   10.42.1.8   k3d-my-cluster-agent-2    <none>           <none>
pod/nginx-deployment-66b6c48dd5-5ghtm   1/1     Running   0          3m47s   10.42.3.6   k3d-my-cluster-agent-1    <none>           <none>
pod/nginx-deployment-66b6c48dd5-fth8g   1/1     Running   0          27s     10.42.1.9   k3d-my-cluster-agent-2    <none>           <none>
pod/nginx-deployment-66b6c48dd5-clq88   1/1     Running   0          27s     10.42.0.5   k3d-my-cluster-server-0   <none>           <none>
pod/nginx-deployment-66b6c48dd5-7plmz   1/1     Running   0          27s     10.42.2.5   k3d-my-cluster-agent-0    <none>           <none>

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE    SELECTOR
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   122m   <none>

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-deployment   5/5     5            5           3m47s   nginx        nginx:1.14.2   app=nginx

NAME                                          DESIRED   CURRENT   READY   AGE     CONTAINERS   IMAGES         SELECTOR
replicaset.apps/nginx-deployment-66b6c48dd5   5         5         5       3m47s   nginx        nginx:1.14.2   app=nginx,pod-template-hash=66b6c48dd5
```

## Clear

```shell
$ kubectl delete -f replica.yaml

deployment.apps "nginx-deployment" deleted
```
