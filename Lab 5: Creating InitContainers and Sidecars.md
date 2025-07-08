# Lab 5: Creating InitContainers and Sidecars


#### Exercise 1: Creating a Pod with InitContainer

**Instructions:**

1. Create a YAML file named `pod-with-init.yaml`:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-init-demo
    spec:
      initContainers:
      - name: init-myservice
        image: busybox
        command: ['sh', '-c', 'echo Initializing...; sleep 5']
      containers:
      - name: main-container
        image: nginx
        ports:
        - containerPort: 80
    ```

2. Apply the manifest to create the Pod:

    ```bash
    kubectl apply -f pod-with-init.yaml
    kubectl get pods
    kubectl describe pod pod-init-demo
    ```

#### Exercise 2: Monitor InitContainer Execution

**Instructions:**

1. Check the Pod's events and logs to observe InitContainer behavior:

    ```bash
    kubectl get pods -w
    kubectl logs pod-init-demo -c init-myservice
    kubectl describe pod pod-init-demo
    ```

#### Exercise 3: Creating a Pod with Sidecar Container

**Instructions:**

1. Create a YAML file named `pod-with-sidecar.yaml`:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-sidecar-demo
    spec:
      containers:
      - name: main-app
        image: nginx
        ports:
        - containerPort: 80
      - name: sidecar-logger
        image: busybox
        command: ['sh', '-c', 'while true; do echo Sidecar running; sleep 5; done']
    ```

2. Apply the manifest to create the Pod:

    ```bash
    kubectl apply -f pod-with-sidecar.yaml
    kubectl get pods
    kubectl describe pod pod-sidecar-demo
    ```

#### Exercise 4: Monitor Sidecar Logs

**Instructions:**

1. Check the logs of the sidecar container:

    ```bash
    kubectl logs pod-sidecar-demo -c sidecar-logger
    ```

2. Observe the interaction (if any) between the main container and the sidecar:

    ```bash
    kubectl logs pod-sidecar-demo -c main-app
    ```
