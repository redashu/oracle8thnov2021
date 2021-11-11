# k8s revision 

<img src="rev.png">

### checking connection to master 

```
kubectl  get  nodes
NAME            STATUS   ROLES                  AGE   VERSION
control-plane   Ready    control-plane,master   25h   v1.22.3
node1           Ready    <none>                 25h   v1.22.3
node2           Ready    <none>                 25h   v1.22.3

```

### checking pod status 

```
 kubectl  get  po   
NAME          READY   STATUS    RESTARTS      AGE
abhishpod1    1/1     Running   1 (17h ago)   17h
amitpod-1     1/1     Running   1 (17h ago)   17h
anushapod-1   1/1     Running   1 (17h ago)   17h

```

### auto generate yaml 

```
kubectl  run  ashupod2 --image=nginx --port 80  --dry-run=client -o yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod2
  name: ashupod2
spec:
  containers:
  - image: nginx
    name: ashupod2
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### save output in a file 

```
 kubectl  run  ashupod2 --image=nginx --port 80  --dry-run=client -o yaml >pod2.yaml
```

## Understanding namespace in k8s 

<img src="ns.png">

### list of namespades 

```
kubectl  get  ns
NAME                   STATUS   AGE
default                Active   25h
kube-node-lease        Active   25h
kube-public            Active   25h
kube-system            Active   25h
kubernetes-dashboard   Active   25h
```

### k8s components are running as pod 

```
kubectl  get  po -n kube-system
NAME                                       READY   STATUS    RESTARTS      AGE
calico-kube-controllers-5d995d45d6-gggbt   1/1     Running   2 (17h ago)   25h
calico-node-fl5t8                          1/1     Running   2 (17h ago)   25h
calico-node-qx5hh                          1/1     Running   2 (17h ago)   25h
calico-node-r5r9j                          1/1     Running   2 (17h ago)   25h
coredns-78fcd69978-d4rbj                   1/1     Running   2 (17h ago)   25h
coredns-78fcd69978-jg78s                   1/1     Running   2 (17h ago)   25h
etcd-control-plane                         1/1     Running   2 (17h ago)   25h
kube-apiserver-control-plane               1/1     Running   2 (17h ago)   25h
kube-controller-manager-control-plane      1/1     Running   2 (17h ago)   25h
kube-proxy-cbhcm                           1/1     Running   2 (17h ago)   25h
kube-proxy-jvlxz                           1/1     Running   2 (17h ago)   25h
kube-proxy-xzrsm                           1/1     Running   2 (17h ago)   25h
kube-scheduler-control-plane               1/1     Running   2 (17h ago)   25h
metrics-server-6fb5c69669-zsg2q            1/1     Running   2 (17h ago)   25h

```

### creating namespaces 

```
kubectl  create  namespace  ashu-space                           
namespace/ashu-space created
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  ns                                                 
NAME                   STATUS   AGE
ashu-space             Active   5s
default                Active   25h
kube-node-lease        Active   25h
kube-public            Active   25h
kube-system            Active   25h
kubernetes-dashboard   Active   25h
rajuspace              Active   33s

```

### settting default namespace 

```
kubectl  config set-context --current --namespace=ashu-space 
Context "kubernetes-admin@kubernetes" modified.
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  po                                             
No resources found in ashu-space namespace.
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  config  get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space

```

###  task done 

```
 kubectl  run ashupod1  --image=busybox --command ping fb.com -n tasks 
 6388  kubectl  get  po -n tasks 
 6389  kubectl  logs  ashupod1  -n tasks 
 6390  kubectl  logs  ashupod1  -n tasks  >logs.txt
 6391  ls
 6392  kubectl  exec -it  ashupod1 -n tasks -- sh 
 6393  kubectl cp logs.txt  ashupod1:/opt/ -n tasks 
 6394  kubectl  exec -it  ashupod1 -n tasks -- sh 
 6395  kubectl  get  po -n tasks -o wide
 6396  kubectl  exec -it  ashupod1 -n tasks -- sh 
 
 ```
 
 
