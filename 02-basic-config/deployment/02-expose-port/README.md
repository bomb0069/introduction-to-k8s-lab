# Mininum Required for Deployment

## Run

```shell
$ kubectl apply -f container-port.yaml

deployment.apps/nginx-deployment configured
```

## Check

list all objects

```shell
$ kubectl get all -o wide

NAME                                    READY   STATUS    RESTARTS   AGE   IP          NODE                     NOMINATED NODE   READINESS GATES
pod/nginx-deployment-66b6c48dd5-82xbw   1/1     Running   0          13s   10.42.1.6   k3d-my-cluster-agent-2   <none>           <none>

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE   SELECTOR
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   17m   <none>

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-deployment   1/1     1            1           13s   nginx        nginx:1.14.2   app=nginx

NAME                                          DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES         SELECTOR
replicaset.apps/nginx-deployment-66b6c48dd5   1         1         1       13s   nginx        nginx:1.14.2   app=nginx,pod-template-hash=66b6c48dd5
```

## change configuration

```shell
$ kubectl apply -f change.yaml

deployment.apps/nginx-deployment configured
```

```shell
$ kubectl get all -o wide     

NAME                                   READY   STATUS    RESTARTS   AGE   IP          NODE                     NOMINATED NODE   READINESS GATES
pod/nginx-deployment-64bd7b69c-tttzg   1/1     Running   0          2s    10.42.3.5   k3d-my-cluster-agent-1   <none>           <none>

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE   SELECTOR
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   19m   <none>

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE    CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-deployment   1/1     1            1           118s   nginx        nginx:1.14.2   app=nginx

NAME                                          DESIRED   CURRENT   READY   AGE    CONTAINERS   IMAGES         SELECTOR
replicaset.apps/nginx-deployment-64bd7b69c    1         1         1       2s     nginx        nginx:1.14.2   app=nginx,pod-template-hash=64bd7b69c
replicaset.apps/nginx-deployment-66b6c48dd5   0         0         0       118s   nginx        nginx:1.14.2   app=nginx,pod-template-hash=66b6c48dd5
```

describe pod

```shell
$ kubectl describe pod nginx-deployment-64bd7b69c-tttzg

Name:         nginx-deployment-64bd7b69c-tttzg
Namespace:    default
Priority:     0
Node:         k3d-my-cluster-agent-1/192.168.16.5
Start Time:   Tue, 08 Feb 2022 22:34:08 +0700
Labels:       app=nginx
              pod-template-hash=64bd7b69c
Annotations:  <none>
Status:       Running
IP:           10.42.3.5
IPs:
  IP:           10.42.3.5
Controlled By:  ReplicaSet/nginx-deployment-64bd7b69c
Containers:
  nginx:
    Container ID:   containerd://7d3ff7cd87643c3a406b2a7c7a93a63073b70034998b404677273101d7a023df
    Image:          nginx:1.14.2
    Image ID:       docker.io/library/nginx@sha256:f7988fb6c02e0ce69257d9bd9cf37ae20a60f1df7563c3a2a6abe24160306b8d
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 08 Feb 2022 22:34:08 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-gnlp4 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-gnlp4:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m49s  default-scheduler  Successfully assigned default/nginx-deployment-64bd7b69c-tttzg to k3d-my-cluster-agent-1
  Normal  Pulled     2m49s  kubelet            Container image "nginx:1.14.2" already present on machine
  Normal  Created    2m49s  kubelet            Created container nginx
  Normal  Started    2m49s  kubelet            Started container nginx
```

## Clear

```shell
$ kubectl delete -f container-port.yaml
```
