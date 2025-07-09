# Lab 8: Working with Secrets

#### Exercise 1: Creating a Secret from Literal Values

**Objective:** Create a Secret named `secret-demo` with sensitive data.

Create a Secret with key-value pairs:

```bash
kubectl create secret generic secret-demo --from-literal=username=admin --from-literal=password=secret
```

Display its values (base64 encoded):

```bash
kubectl get secret secret-demo -o yaml
```

Decode the values:

```bash
echo "YWRtaW4=" | base64 --decode
echo "c2VjcmV0" | base64 --decode
```

#### Exercise 2: Load Secret as Env Variables in a Pod

**Objective:** Use a Secret as environment variables in a Pod.

Create a YAML file named `pod-secret-env.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-secret-env-demo
spec:
  containers:
  - name: secret-container
    image: busybox
    command: ['sh', '-c', 'echo Username: $USERNAME, Password: $PASSWORD; sleep 3600']
    env:
    - name: USERNAME
      valueFrom:
        secretKeyRef:
          name: secret-demo
          key: username
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: secret-demo
          key: password
```

Apply and verify:

```bash
kubectl apply -f pod-secret-env.yaml
kubectl logs pod-secret-env-demo
```


#### Exercise 3: Load Secret as a Volume

**Objective:** Use a Secret as a volume in a Pod.


Create a YAML file named `pod-secret-volume.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-secret-volume-demo
spec:
  containers:
  - name: secret-volume-container
    image: busybox
    command: ['sh', '-c', 'ls /etc/secret; cat /etc/secret/username; cat /etc/secret/password; sleep 3600']
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/secret
  volumes:
  - name: secret-volume
    secret:
      secretName: secret-demo
```

Apply and verify:

```bash
kubectl apply -f pod-secret-volume.yaml
kubectl logs pod-secret-volume-demo
```

#### Exercise 4: Access and Utilize Secret Data

**Objective:** Access and utilize secret data within a running container in a Pod.

1. **Create a Secret with sensitive data:**

    ```bash
    kubectl create secret generic access-secret --from-literal=token=abc123
    ```

2. **Create a Pod to access the Secret data:**

    Create a YAML file named `pod-access-secret.yaml`:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-access-secret-demo
    spec:
      containers:
      - name: access-container
        image: busybox
        command: ['sh', '-c', 'echo Access Token: $TOKEN; sleep 3600']
        env:
        - name: TOKEN
          valueFrom:
            secretKeyRef:
              name: access-secret
              key: token
    ```

3. **Apply the manifest to create the Pod:**

    ```bash
    kubectl apply -f pod-access-secret.yaml
    ```

4. **Verify the access to Secret data:**

    View the logs to ensure the Secret data is accessed correctly:

    ```bash
    kubectl logs pod-access-secret-demo
    ```

5. **View the Secret in its encoded form:**

    ```bash
    kubectl get secret access-secret -o yaml
    ```

6. **Decode the Secret manually:**

    Verify by decoding the base64 value manually:

    ```bash
    echo "YWJjMTIz" | base64 --decode
