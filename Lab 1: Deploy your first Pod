# Lab 1: Deploy your first Pod

Objective: Learn basic pod creation and management.


#### Exercise 1: Basic Pod Creation

**Instructions:**

1. Create a YAML file named `pod-nginx-dev.yaml` with the following content:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-nginx-dev
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
    ```

2. Apply the manifest to create the pod:

    ```bash
    kubectl apply -f pod-nginx-dev.yaml
    ```

3. Verify the pod creation:

    ```bash
    kubectl get pods
    kubectl describe pod pod-nginx-dev
    kubectl logs pod-nginx-dev
    ```

#### Exercise 2: Applying Labels

**Instructions:**

1. Create a YAML file named `pod-web-dev.yaml` with labels:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-nginx-dev
      labels:
        app: web
        env: dev
    spec:
      containers:
      - name: nginx
        image: nginx
    ```

2. Apply the manifest:

    ```bash
    kubectl apply -f pod-web-dev.yaml
    ```

3. View pods with labels:

    ```bash
    kubectl get pods --show-labels
    ```

#### Exercise 3: Working with Annotations

**Instructions:**

1. Create a YAML file named `pod-nginx-info.yaml` with annotations:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-nginx-dev
      annotations:
        description: "Sample Nginx pod"
        version: "1.0"
        maintainer: "example@example.com"
    spec:
      containers:
      - name: nginx
        image: nginx
    ```

2. Apply the manifest:

    ```bash
    kubectl apply -f pod-nginx-info.yaml
    ```

3. Check annotations:

    ```bash
    kubectl describe pod pod-nginx-info
    ```
