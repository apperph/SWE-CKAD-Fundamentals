# Lab 13: Performing Canary Deployment

#### Exercise 1: Implement a Canary Deployment

**Objective:** Gradually introduce a new version of the application alongside the stable version.

1. **Create the stable Deployment:**

    Create a YAML file named `app-stable.yaml`:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: app-stable
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: myapp
          version: stable
      template:
        metadata:
          labels:
            app: myapp
            version: stable
        spec:
          containers:
          - name: app-container
            image: nginx:1.17
            command: ['sh', '-c', 'echo "Welcome to the Stable version!" > /usr/share/nginx/html/index.html; nginx -g "daemon off;"']
            ports:
            - containerPort: 80
    ```

2. **Apply the stable Deployment:**

    ```bash
    kubectl apply -f app-stable.yaml
    kubectl get deployments
    kubectl get pods
    ```

3. **Create the canary Deployment:**

    Create a YAML file named `app-canary.yaml`:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: app-canary
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: myapp
          version: canary
      template:
        metadata:
          labels:
            app: myapp
            version: canary
        spec:
          containers:
          - name: app-container
            image: httpd:2.4
            command: ['sh', '-c', 'echo "Welcome to the Canary version!" > /usr/local/apache2/htdocs/index.html; httpd-foreground']
            ports:
            - containerPort: 80
    ```

4. **Apply the canary Deployment:**

    ```bash
    kubectl apply -f app-canary.yaml
    kubectl get deployments
    kubectl get pods
    ```

5. **Create a Service to serve both stable and canary:**

    ```bash
    kubectl expose deployment app-stable --port=80 --target-port=80 --name=myapp-canary-service
    ```

6. **Patch the service selector to include both versions:**

    ```bash
    kubectl patch svc myapp-canary-service -p '{"spec":{"selector":{"app":"myapp"}}}'
    ```

7. **Test the Service by accessing it:**

    Use `kubectl port-forward`:

    ```bash
    kubectl port-forward svc/myapp-canary-service 8080:80
    curl http://localhost:8080
    ```

8. **Scale canary deployment to increase traffic proportion:**

    ```bash
    kubectl scale deployment app-canary --replicas=2
    kubectl get pods
    ```

9. **Test the Service again to ensure canary deployment is active:**

    ```bash
    curl http://localhost:8080
    ```
