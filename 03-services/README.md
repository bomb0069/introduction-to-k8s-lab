# Service

List of All Service in K8s

```shell
NAME                    TYPE            CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE    SELECTOR
kubernetes              ClusterIP       10.43.0.1      <none>           443/TCP        3m2s   <none>
service-cluster-ip      ClusterIP       10.43.25.152   <none>           80/TCP         71s    app=my-web
service-headless        ClusterIP       None           <none>           80/TCP         71s    app=nginx
service-node-port       NodePort        10.43.212.126  <none>           80:30080/TCP   22s   app=my-web
service-load-balancer   LoadBalancer    10.43.220.227  172.29.0.2,...   80:30080/TCP   51s   app=my-web
```