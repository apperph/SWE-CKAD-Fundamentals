# Lab 10: Managing Resource Limits and Quotas

### Overview

This lab focuses on setting resource limits for Pods and implementing resource quotas at the namespace level.

#### Exercise 1: Setting Resource Requests and Limits

**Objective:** Configure resource requests and limits for a Pod to manage CPU and memory usage.


Create a YAML file named `pod-resource-limits.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-resource-demo
spec:
  containers:
  - name: resource-container
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

Apply and verify:

```bash
kubectl apply -f pod-resource-limits.yaml
kubectl describe pod pod-resource-demo
```

### 2. Resource Quotas

#### Exercise 2: Creating a Resource Quota

**Objective:** Enforce resource usage policies on a namespace by setting a ResourceQuota.


Create a YAML file named `resource-quota.yaml`:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: <your-namespace>
spec:
  hard:
    pods: "5"
    requests.cpu: "1"
    requests.memory: "1Gi"
    limits.cpu: "2"
    limits.memory: "2Gi"
```

Apply and verify:

```bash
kubectl apply -f resource-quota.yaml
kubectl describe resourcequota compute-quota -n <your-namespace>
```

Create a Pod in your namespace and observe quota enforcement:

```bash
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: <your-namespace>
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        cpu: "200m"
        memory: "100Mi"
      limits:
        cpu: "500m"
        memory: "200Mi"
```

### 3. Advanced Exercises

#### Exercise 3: Exceeding Resource Quotas

**Objective:** Attempt to exceed a ResourceQuota and observe the behavior.

Try creating Pods to exceed the quota:

```bash
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: <your-namespace>
spec:
  containers:
  - name: busybox
    image: busybox
    resources:
      requests:
        cpu: "700m"
        memory: "700Mi"
      limits:
        cpu: "900m"
        memory: "900Mi"
    command: ['sh', '-c', 'echo Hello Kubernetes!; sleep 3600']
  restartPolicy: Never
```

Observe the result.
