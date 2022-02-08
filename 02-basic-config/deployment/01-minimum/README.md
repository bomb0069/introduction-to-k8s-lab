# Mininum Required for Deployment

## Run

```shell
$ kubectl apply -f deployment.yaml

deployment.apps/nginx-deployment configured
```

## Check

list all objects

```shell
$ kubectl get all
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   1/1     1            1           41s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-877f48f6d   1         1         1       41s

NAME                                   READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-877f48f6d-mj7s2   1/1     Running   0          41s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   3m59s
```

describe deployment

```shell
$ kubectl describe deployment nginx-deployment

Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Tue, 08 Feb 2022 22:18:05 +0700
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.14.2
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-877f48f6d (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  4m30s  deployment-controller  Scaled up replica set nginx-deployment-877f48f6d to 1
```

```shell
$ kubectl describe replicaset nginx-deployment-877f48f6d

Name:           nginx-deployment-877f48f6d
Namespace:      default
Selector:       app=nginx,pod-template-hash=877f48f6d
Labels:         app=nginx
                pod-template-hash=877f48f6d
Annotations:    deployment.kubernetes.io/desired-replicas: 1
                deployment.kubernetes.io/max-replicas: 2
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/nginx-deployment
Replicas:       1 current / 1 desired
Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
           pod-template-hash=877f48f6d
  Containers:
   nginx:
    Image:        nginx:1.14.2
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age    From                   Message
  ----    ------            ----   ----                   -------
  Normal  SuccessfulCreate  4m55s  replicaset-controller  Created pod: nginx-deployment-877f48f6d-mj7s2
```

describe pod

```shell
$ kubectl describe pod nginx-deployment-877f48f6d-mj7s2

Name:         nginx-deployment-877f48f6d-mj7s2
Namespace:    default
Priority:     0
Node:         k3d-my-cluster-agent-2/192.168.16.3
Start Time:   Tue, 08 Feb 2022 22:18:05 +0700
Labels:       app=nginx
              pod-template-hash=877f48f6d
Annotations:  <none>
Status:       Running
IP:           10.42.1.5
IPs:
  IP:           10.42.1.5
Controlled By:  ReplicaSet/nginx-deployment-877f48f6d
Containers:
  nginx:
    Container ID:   containerd://5a433c4cfbcf9d4a7208904f3f5ffd6bfade29a46fe4519ea17fd33df03855d0
    Image:          nginx:1.14.2
    Image ID:       docker.io/library/nginx@sha256:f7988fb6c02e0ce69257d9bd9cf37ae20a60f1df7563c3a2a6abe24160306b8d
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 08 Feb 2022 22:18:06 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-twgnx (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-twgnx:
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
  Normal  Scheduled  5m13s  default-scheduler  Successfully assigned default/nginx-deployment-877f48f6d-mj7s2 to k3d-my-cluster-agent-2
  Normal  Pulled     5m13s  kubelet            Container image "nginx:1.14.2" already present on machine
  Normal  Created    5m13s  kubelet            Created container nginx
  Normal  Started    5m13s  kubelet            Started container nginx
```

## Clear

```shell
$ kubectl delete -f deployment.yaml
```
