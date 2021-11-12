# final day 

### building image from github URL 

```
 docker  build -t dockerashu/tomcat:v0011  https://github.com/redashu/javawebapp.git
 
 ```
 
 ### list of api resources 
 
 ```
 kubectl api-resources 
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v

```

### Deployment again 

<img src="deployment.png">

### deploy the deployment in current namespace 

```
kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space

```

### creating deployment 

```
 kubectl apply -f  tomcat.yaml 
deployment.apps/tomcatapp created
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get deploy
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
tomcatapp   1/1     1            1           18s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  rs   
NAME                   DESIRED   CURRENT   READY   AGE
tomcatapp-7bd98b85d6   1         1         1       25s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  po
NAME                         READY   STATUS    RESTARTS   AGE
tomcatapp-7bd98b85d6-q2szv   1/1     Running   0          32s

```

### creating svc 

```
kubectl expose deployment tomcatapp --type LoadBalancer   --port 8080 --name  ashusvc1 --dry-run=client  -o yaml 
 6559  kubectl expose deployment tomcatapp --type LoadBalancer   --port 8080 --name  ashusvc1
 
```
 
### checking revision number in deployment 

<img src="rev.png">

### scaling concept of pod via  RC | RS | Deployment 

<img src="scale.png">

### manual horizental scaling of pod 

```
 kubectl  get deploy 
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
tomcatapp   1/1     1            1           9m21s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl scale deployment tomcatapp --replicas=3
deployment.apps/tomcatapp scaled
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get deploy                            
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
tomcatapp   3/3     3            3           15m
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get po -o wide
NAME                         READY   STATUS    RESTARTS   AGE   IP               NODE    NOMINATED NODE   READINESS GATES
tomcatapp-7bd98b85d6-4b49t   1/1     Running   0          12s   192.168.104.63   node2   <none>           <none>
tomcatapp-7bd98b85d6-m6rkn   1/1     Running   0          12s   192.168.104.51   node2   <none>           <none>
tomcatapp-7bd98b85d6-q2szv   1/1     Running   0          15m   192.168.104.48   node2   <none> 

```

### updating new image to deployment 

```
kubectl  set  image  deployment tomcatapp              tomcat=dockerashu/tomcat:v0022 

```

### checking revision number 

```
 kubectl describe deploy  tomcatapp         Name:                   tomcatapp
Namespace:              ashu-space
CreationTimestamp:      Fri, 12 Nov 2021 10:13:04 +0530
Labels:                 app=tomcatapp
Annotations:            deployment.kubernetes.io/revision: 2
Selector:               app=tomcatapp
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=tomcatapp
  Containers:
   tomcat:
    Image:        dockerashu/tomcat:v0022
    
```

### rolling back to prevision version 

```

 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  rollout undo  deployment  tomcatapp
deployment.apps/tomcatapp rolled back
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  rollout status deployment  tomcatapp 
Waiting for deployment "tomcatapp" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "tomcatapp" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "tomcatapp" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "tomcatapp" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "tomcatapp" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "tomcatapp" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "tomcatapp" rollout to finish: 1 old replicas are pending termination...
deployment "tomcatapp" successfully rolled out

```

## horizental pod autoscaler (HPA)

<img src="hpa.png">

### putting limit in cpu 

<img src="cpul.png">

### implementing hpa 

```
 kubectl  get  hpa
NAME        REFERENCE              TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
tomcatapp   Deployment/tomcatapp   1%/80%    3         10        3          32s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get po 
NAME                         READY   STATUS    RESTARTS   AGE
tomcatapp-66566c6976-kbbds   1/1     Running   0          2m15s
tomcatapp-66566c6976-sn7p2   1/1     Running   0          27s
tomcatapp-66566c6976-vhztr   1/1     Running   0          27s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  hpa
NAME        REFERENCE              TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
tomcatapp   Deployment/tomcatapp   1%/80%    3         10        3          51s
```

### Minikube 

<img src="min.png">

### Installing in Mac os 

```
598  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
 6599  sudo install minikube-darwin-amd64 /usr/local/bin/minikube
 
 minikube  version 
minikube version: v1.24.0
commit: 76b94fb3c4e8ac5062daf70d60cf03ddcc0a741b

```

### setting up k8s cluster 

```
minikube  start  --driver=docker 
😄  minikube v1.24.0 on Darwin 11.6
✨  Using the docker driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.22.3 preload ...
    > gcr.io/k8s-minikube/kicbase: 39.52 MiB / 355.78 MiB  11.11% 2.38 MiB p/s 
    
```

### switching between k8s cluster from client side 

```
 kubectl  config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
          kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
*         minikube                      minikube     minikube           default
 fire@ashutoshhs-MacBook-Air  ~  
 fire@ashutoshhs-MacBook-Air  ~  kubectl  config  use-context  kubernetes-admin@kubernetes
Switched to context "kubernetes-admin@kubernetes".
 fire@ashutoshhs-MacBook-Air  ~  kubectl  config get-contexts                             
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
          minikube                      minikube     minikube           default
 fire@ashutoshhs-MacBook-Air  ~  kubectl  get  no
NAME            STATUS   ROLES                  AGE    VERSION
control-plane   Ready    control-plane,master   2d2h   v1.22.3
node1           Ready    <none>                 2d2h   v1.22.3
node2           Ready    <none>                 2d2h   v1.22.3


```

### storage in k8s  

<img src="st.png">

### volume 

<img src="vol.png">

### emptyDir type volume demo 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod1
  name: ashupod1
spec:
  nodeName: node2 # static scheduling 
  volumes: # to create volume 
  - name: ashuvol11  # name of volume 
    emptyDir: {} # it will take space from minion node 2 
  containers:
  - image: alpine
    name: ashupod1
    volumeMounts: # to mount volume inside container 
    - name: ashuvol11 # name of volume 
      mountPath: /mnt/data # inside container it will be created 
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### checking pod 

```
 kubectl  get po -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP               NODE    NOMINATED NODE   READINESS GATES
ashupod1   1/1     Running   0          91s   192.168.104.16   node2   <none>           <none>
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  exec -it  ashupod1 -- sh 
/ # cd /mnt/data/
/mnt/data # ls
time.txt
/mnt/data # exit
```
### idea behind multi container pod 
<img src="mc.png">

### multi container pod 

```
kubectl apply -f emp1.yaml 
The Pod "ashupod1" is invalid: spec.containers: Forbidden: pod updates may not add or remove containers
 ✘ fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl replace -f emp1.yaml --force
pod "ashupod1" deleted
pod/ashupod1 replaced
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get po                     
NAME       READY   STATUS    RESTARTS   AGE
ashupod1   2/2     Running   0          10s

```

### login into container 
```

 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  exec  -it  ashupod1  -- bash 
Defaulted container "ashuc1" out of: ashuc1, ashupod1
root@ashupod1:/# 
root@ashupod1:/# cd /usr/share/nginx/html/
root@ashupod1:/usr/share/nginx/html# ls
time.txt
root@ashupod1:/usr/share/nginx/html# exit
exit
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  exec  -it  ashupod1 -c ashupod1    -- sh  
/ # cd /mnt/data/
/mnt/data # ls
time.txt
/mnt/data # exit
```

### Exposing pod to create service 

```
 kubectl  expose pod ashupod1  --type NodePort --port  80  
service/ashupod1 exposed
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  svc
NAME       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
ashupod1   NodePort   10.107.148.84   <none>        80:31500/TCP   6s

```

### Dashboard deployment in k8s cluster 

## 

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created

```

### checking details 

```
kubectl  get  deploy -n kubernetes-dashboard 
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
dashboard-metrics-scraper   1/1     1            1           52s
kubernetes-dashboard        1/1     1            1           54s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  po -n kubernetes-dashboard 
NAME                                        READY   STATUS    RESTARTS   AGE
dashboard-metrics-scraper-c45b7869d-q999q   1/1     Running   0          68s
kubernetes-dashboard-576cb95f94-vh7xh       1/1     Running   0          70s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  svc -n kubernetes-dashboard 
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
dashboard-metrics-scraper   ClusterIP   10.111.255.215   <none>        8000/TCP   79s
kubernetes-dashboard        ClusterIP   10.103.80.106    <none>        443/TCP    87s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  secret  -n kubernetes-dashboard 
NAME                               TYPE                                  DATA   AGE
default-token-bqxpt                kubernetes.io/service-account-token   3      103s
kubernetes-dashboard-certs         Opaque                                0      101s
kubernetes-dashboard-csrf          Opaque                                1      100s
kubernetes-dashboard-key-holder    Opaque                                2      99s
kubernetes-dashboard-token-jgqj7   kubernetes.io/service-account-token   3      102s

```

### changing service 

```
 kubectl  edit  svc  kubernetes-dashboard        -n kubernetes-dashboard 
service/kubernetes-dashboard edited
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  svc -n kubernetes-dashboard                           
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
dashboard-metrics-scraper   ClusterIP   10.111.255.215   <none>        8000/TCP        2m38s
kubernetes-dashboard        NodePort    10.103.80.106    <none>        443:32704/TCP   2m46s
```
### giving permission to dashboard to access cluster data 

```
kubectl create  clusterrolebinding  dashboardaccess  --clusterrole=cluster-admin    --serviceaccount=kubernetes-dashboard:kubernetes-dashboard
```

### Dashboard in Minikube based k8s cluster 

```
minikube dashboard  --url
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
http://127.0.0.1:52305/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/



```
## k8s External storage with NFS for Db pod 

<img src="db.png">

## Creating app stack --
### DB yaml 

```
kubectl create  deployment  ashudb --image=mysql:5.6 --dry-run=client -o yaml   >ashuapp.yaml

```

### deploy DB deployment 

```
kubectl apply -f ashuapp.yaml 
deployment.apps/ashudb created
secret/mysec created
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  secrets 
NAME                  TYPE                                  DATA   AGE
default-token-gsqhm   kubernetes.io/service-account-token   3      27h
mysec                 Opaque                                1      7s
ocrsec                kubernetes.io/dockerconfigjson        1      22h
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  deploy  
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
ashudb   1/1     1            1           18s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  po    
NAME                      READY   STATUS    RESTARTS   AGE
ashudb-6f5f9cd5db-7zdc9   1/1     Running   0          24s

```

### creating service 

```
kubectl  expose deployment  ashudb  --type ClusterIP --port 3306 --dry-run=client -o yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashudb
  name: ashudb
spec:
  ports:
  - port: 3306
    protocol: TCP
    targetPort: 3306
  selector:
    app: ashudb
  type: ClusterIP
status:
  loadBalancer: {}

```

### clusterIP type service 

```
kubectl apply -f ashuapp.yaml 
deployment.apps/ashudb configured
secret/mysec configured
service/ashudb created
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get svc
NAME     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
ashudb   ClusterIP   10.98.240.184   <none>        3306/TCP   12s

```

### wordpress 
```
kubectl create deployment  ashuwebapp --image=wordpress:4.8-apache --dry-run=client -o yaml 

```

### wordpress db host 

```
 kubectl apply -f  ashuapp.yaml 
deployment.apps/ashudb configured
secret/mysec configured
service/ashudb configured
deployment.apps/ashuwebapp created
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get deploy 
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
ashudb       1/1     1            1           20m
ashuwebapp   0/1     1            0           6s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get deploy 
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
ashudb       1/1     1            1           20m
ashuwebapp   0/1     1            0           10s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  po    
NAME                          READY   STATUS              RESTARTS   AGE
ashudb-6f5f9cd5db-7zdc9       1/1     Running             0          21m
ashuwebapp-85bc9d67bb-rmc4l   0/1     ContainerCreating   0          15s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl  get  po
NAME                          READY   STATUS    RESTARTS   AGE
ashudb-6f5f9cd5db-7zdc9       1/1     Running   0          21m
ashuwebapp-85bc9d67bb-rmc4l   1/1     Running   0          26s
 fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  


```

### Exposing db 

```
fire@ashutoshhs-MacBook-Air  ~/Desktop/k8s_apps  kubectl expose deploy  ashuwebapp --type NodePort  --port 80 --dry-run=client -o yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashuwebapp
  name: ashuwebapp
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: ashuwebapp
  type: NodePort
status:
  loadBalancer: {}

```

### app with SErvice 

<img src="svc.png">

### Ingress controller is the solution here

<img src="ingress.png">

### deploy ingress controller in baremetal / vm 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/baremetal/deploy.yaml
```

### URL 

[Ingress deploy](https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal)

### Deploy Ingress controller 

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/baremetal/deploy.yaml
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
configmap/ingress-nginx-controller created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
service/ingress-nginx-controller-admission created
service/ingress-nginx-controller created
deployment.apps/ingress-nginx-controller created
ingressclass.networking.k8s.io/nginx created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
serviceaccount/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
```

### Intro to Helm 

<img src="helm.png">



