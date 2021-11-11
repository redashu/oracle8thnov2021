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

