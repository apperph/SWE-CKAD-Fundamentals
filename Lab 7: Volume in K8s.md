## Lab 7: Volumes in K8s

### Overview


### 1. Using emptyDir Volumes

#### Exercise 1: Creating a Pod with emptyDir

**Objective:** Use `emptyDir` for sharing storage between containers in a Pod.

Create a YAML file named `pod-emptydir.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-emptydir-demo
spec:
  containers:
  - name: main-container
    image: busybox
    command: ['sh', '-c', 'echo "Hello from main-container" > /data/message; sleep 3600']
    volumeMounts:
    - mountPath: /data
      name: shared-data
  - name: sidecar-container
    image: busybox
    command: ['sh', '-c', 'while true; do cat /data/message; sleep 5; done']
    volumeMounts:
    - mountPath: /data
      name: shared-data
  volumes:
  - name: shared-data
    emptyDir: {}
```

Apply and verify:

```bash
kubectl apply -f pod-emptydir.yaml
kubectl logs pod-emptydir-demo -c sidecar-container --tail=5
```

#### Exercise 2: Creating a Pod with hostPath

**Objective:** Use `hostPath` to mount a directory from the host node’s filesystem into your Pod.

Create a YAML file named `pod-hostpath.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-hostpath-demo
spec:
  containers:
  - name: hostpath-container
    image: busybox
    command: ['sh', '-c', 'touch /host/data.txt; ls /host; sleep 3600']
    volumeMounts:
    - mountPath: /host
      name: host-volume
  volumes:
  - name: host-volume
    hostPath:
      path: /tmp/host-demo
      type: DirectoryOrCreate
```

Apply and verify:

```bash
kubectl apply -f pod-hostpath.yaml
kubectl exec -it pod-hostpath-demo -- ls /host
```


#### Exercise 3: Creating PV and PVC

**Objective:** Use `PersistentVolume` and `PersistentVolumeClaim` to manage storage that persists beyond the lifecycle of a Pod.

Create a YAML file named `pv.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-demo
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /tmp/pv-demo
```

Create a YAML file named `pvc.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

Create a Pod using PVC:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-pv-demo
spec:
  containers:
  - name: pvc-container
    image: busybox
    command: ['sh', '-c', 'echo "Persisted data" > /persistent/data.txt; sleep 3600']
    volumeMounts:
    - mountPath: /persistent
      name: pv-storage
  volumes:
  - name: pv-storage
    persistentVolumeClaim:
      claimName: pvc-demo
```

Apply PV, PVC, and Pod manifests:

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f pod-pv-demo.yaml
kubectl exec -it pod-pv-demo -- cat /persistent/data.txt
```

Add more data:
```
kubectl exec pod-pv-demo -- sh -c 'echo "Additional persisted data" >> /persistent/data.txt'
kubectl exec -it pod-pv-demo -- cat /persistent/data.txt
```
