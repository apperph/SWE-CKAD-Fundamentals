# Lab 9: Creating Service Account

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure Service Accounts for Pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

### Exercise 1: See all the service accounts of the cluster in all namespaces


```bash
kubectl get sa --all-namespaces
```
Alternatively 

```bash
kubectl get sa -A
```


### Exercise 2: Create a new serviceaccount called 'new-user-<initials>'

```bash
kubectl create sa new-user-<initials>
```

Alternatively:

```bash
# let's get a template easily
kubectl get sa default -o yaml > sa.yaml
vim sa.yaml
```

```YAML
apiVersion: v1
kind: ServiceAccount
metadata:
  name: new-user-<initials>
```

```bash
kubectl create -f sa.yaml
```

### Exercise 3: Create an nginx pod that uses 'myuser' as a service account


```bash
kubectl run nginx --image=nginx --restart=Never -o yaml --dry-run=client > pod.yaml
vi pod.yaml
```

```YAML
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  serviceAccountName: myuser # we use pod.spec.serviceAccountName
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

or

```YAML
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  serviceAccount: myuser # we use pod.spec.serviceAccount
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```bash
kubectl create -f pod.yaml
kubectl describe pod nginx # will see that a new secret called myuser-token-***** has been mounted
```


### Exercise 4: Generate an API token for the service account 'myuser'

  
```bash
kubectl create token myuser
```
