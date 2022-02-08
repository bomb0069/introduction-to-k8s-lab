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

NAME                                   READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-877f48f6d-dhbh5   1/1     Running   0          7s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   32m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   1/1     1            1           7s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-877f48f6d   1         1         1       7s
```

### Get deployment configuration

```shell
$ kubectl get deployment nginx-deployment -o yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-deployment","namespace":"default"},"spec":{"selector":{"matchLabels":{"app":"nginx"}},"template":{"metadata":{"labels":{"app":"nginx"}},"spec":{"containers":[{"image":"nginx:1.14.2","name":"nginx"}]}}}}
  creationTimestamp: "2022-02-08T15:47:29Z"
  generation: 1
  name: nginx-deployment
  namespace: default
  resourceVersion: "1984"
  uid: 6040fc0f-bce8-4470-aa09-8f6821d15c9f
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: nginx
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.14.2
        imagePullPolicy: IfNotPresent
        name: nginx
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2022-02-08T15:47:29Z"
    lastUpdateTime: "2022-02-08T15:47:29Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2022-02-08T15:47:29Z"
    lastUpdateTime: "2022-02-08T15:47:29Z"
    message: ReplicaSet "nginx-deployment-877f48f6d" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
```

### Get replicaset configuration

```shell
$ kubectl get replicaset nginx-deployment-877f48f6d -o yaml

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  annotations:
    deployment.kubernetes.io/desired-replicas: "1"
    deployment.kubernetes.io/max-replicas: "2"
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2022-02-08T15:47:29Z"
  generation: 1
  labels:
    app: nginx
    pod-template-hash: 877f48f6d
  name: nginx-deployment-877f48f6d
  namespace: default
  ownerReferences:
  - apiVersion: apps/v1
    blockOwnerDeletion: true
    controller: true
    kind: Deployment
    name: nginx-deployment
    uid: 6040fc0f-bce8-4470-aa09-8f6821d15c9f
  resourceVersion: "1983"
  uid: 6b6abea6-55ec-4ea8-b44d-6372a6ddf592
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
      pod-template-hash: 877f48f6d
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
        pod-template-hash: 877f48f6d
    spec:
      containers:
      - image: nginx:1.14.2
        imagePullPolicy: IfNotPresent
        name: nginx
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  fullyLabeledReplicas: 1
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
```

### Get Configuration of Pod

```shell
$ kubectl get pod nginx-deployment-877f48f6d-dhbh5 -o yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2022-02-08T15:47:29Z"
  generateName: nginx-deployment-877f48f6d-
  labels:
    app: nginx
    pod-template-hash: 877f48f6d
  name: nginx-deployment-877f48f6d-dhbh5
  namespace: default
  ownerReferences:
  - apiVersion: apps/v1
    blockOwnerDeletion: true
    controller: true
    kind: ReplicaSet
    name: nginx-deployment-877f48f6d
    uid: 6b6abea6-55ec-4ea8-b44d-6372a6ddf592
  resourceVersion: "1982"
  uid: 5ab05dd3-586e-4aea-9088-47e13ab546bc
spec:
  containers:
  - image: nginx:1.14.2
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-ntbv4
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: k3d-my-cluster-agent-2
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-ntbv4
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2022-02-08T15:47:29Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2022-02-08T15:47:29Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2022-02-08T15:47:29Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2022-02-08T15:47:29Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://218f3ad96ce431ffd2440f2310159f1e7c2da697534a2cc278d76d5f94632516
    image: docker.io/library/nginx:1.14.2
    imageID: docker.io/library/nginx@sha256:f7988fb6c02e0ce69257d9bd9cf37ae20a60f1df7563c3a2a6abe24160306b8d
    lastState: {}
    name: nginx
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2022-02-08T15:47:29Z"
  hostIP: 192.168.16.3
  phase: Running
  podIP: 10.42.1.7
  podIPs:
  - ip: 10.42.1.7
  qosClass: BestEffort
  startTime: "2022-02-08T15:47:29Z"
```

## Clear

```shell
$ kubectl delete -f deployment.yaml
```
