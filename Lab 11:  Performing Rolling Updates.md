# Lab 11:  Performing Rolling Updates

### Overview

This session covers performing rolling updates with Kubernetes Deployments to ensure zero-downtime updates of applications.

#### Exercise 1: Create a Deployment

**Objective:** Deploy an application that we can update.

Create a YAML file named `deployment-v1.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rolling-update-demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: rolling-demo
  template:
    metadata:
      labels:
        app: rolling-demo
    spec:
      containers:
      - name: demo-container
        image: nginx:1.17
        ports:
        - containerPort: 80
```

Apply the Deployment:

```bash
kubectl apply -f deployment-v1.yaml
kubectl get deployments
kubectl get pods
```


### 2. Performing a Rolling Update

#### Exercise 2: Update the Deployment to a New Version

**Objective:** Update the Deployment to use a newer version of the application.


Update the image in the Deployment:

```bash
kubectl set image deployment/rolling-update-demo demo-container=nginx:1.18 --record
```

Check the rollout status:

```bash
kubectl rollout status deployment/rolling-update-demo
```

Verify the update:

```bash
kubectl get pods
kubectl describe deployment/rolling-update-demo
```


#### Exercise 3: Roll Back the Deployment

**Objective:** Roll back to the previous version if issues arise.


Roll back the Deployment:

```bash
kubectl rollout undo deployment/rolling-update-demo
```

Check the rollout status:

```bash
kubectl rollout status deployment/rolling-update-demo
```

Verify the rollback:

```bash
kubectl get pods
kubectl describe deployment/rolling-update-demo
```


### 4. Viewing Rollout History

#### Exercise 4: Examine the Rollout History

**Objective:** View the history of changes made to the Deployment.

View the rollout history:

```bash
kubectl rollout history deployment/rolling-update-demo
```

View details of a specific revision (if applicable):

```bash
kubectl rollout history deployment/rolling-update-demo --revision=2
```
