## Lab 2: Understanding ReplicaSets

#### Exercise 1: Creating a ReplicaSet

**Instructions:**

1. Create a YAML file named `rs-web.yaml` with the following content:

    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: rs-web
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: web
      template:
        metadata:
          labels:
            app: web
        spec:
          containers:
          - name: nginx
            image: nginx
    ```

2. Apply the manifest to create the ReplicaSet:

    ```bash
    kubectl apply -f rs-web.yaml
    kubectl get rs
    kubectl get pods
    ```

Method 2: Using `kubectl create` Command**

1. Create a ReplicaSet using the command line:

    ```bash
    kubectl create deploy rs-web --image=nginx --replicas=3 --dry-run=client -o yaml > rs-web.yaml
    kubectl apply -f rs-web.yaml
    kubectl get rs
    kubectl get pods
    ```

#### Exercise 2: Observing ReplicaSet Behavior

**Instructions:**

1. Delete a pod and watch how the ReplicaSet reacts:

    ```bash
    kubectl delete pod <pod-name>
    kubectl get pods
    ```

2. Scale the ReplicaSet to five replicas and observe:

    ```bash
    kubectl scale rs rs-web --replicas=5
    kubectl get pods
    ```

#### Exercise 3: Consistent Labeling

**Instructions:**

1. Create a YAML file named `rs-app3.yaml` with consistent labels:

    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: rs-app3
    spec:
      replicas: 5
      selector:
        matchLabels:
          app: app3
      template:
        metadata:
          labels:
            app: app3
        spec:
          containers:
          - name: app3
            image: nginx
    ```

2. Apply the manifest and verify the labels:

    ```bash
    kubectl apply -f rs-app3.yaml
    kubectl get pods --show-labels
    ```
