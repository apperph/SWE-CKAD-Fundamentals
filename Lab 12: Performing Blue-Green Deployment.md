Lab 12: Performing Blue-Green Deployment

### Overview

This lab covers performing Blue/Green deployment to manage application updates with reduced risk.

#### Exercise 1: Implement a Blue/Green Deployment

**Objective:** Deploy two versions of an application and switch traffic between them.


1. **Create the "blue" Deployment:**

    Create a YAML file named `app-blue.yaml`:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: app-blue
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: myapp
          version: blue
      template:
        metadata:
          labels:
            app: myapp
            version: blue
        spec:
          containers:
          - name: app-container
            image: nginx:1.17
            command: ['sh', '-c', 'echo "Welcome to the Blue version!" > /usr/share/nginx/html/index.html; nginx -g "daemon off;"']
            ports:
            - containerPort: 80
    ```

2. **Apply the "blue" Deployment:**

    ```bash
    kubectl apply -f app-blue.yaml
    kubectl get deployments
    kubectl get pods
    ```

3. **Create the "green" Deployment:**

    Create a YAML file named `app-green.yaml`:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: app-green
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: myapp
          version: green
      template:
        metadata:
          labels:
            app: myapp
            version: green
        spec:
          containers:
          - name: app-container
            image: httpd:2.4
            command: ['sh', '-c', 'echo "Welcome to the Green version!" > /usr/local/apache2/htdocs/index.html; httpd-foreground']
            ports:
            - containerPort: 80
    ```

4. **Apply the "green" Deployment:**

    ```bash
    kubectl apply -f app-green.yaml
    kubectl get deployments
    kubectl get pods
    ```

5. **Create a Service to switch between "blue" and "green":**

    ```bash
    kubectl expose deployment app-blue --port=80 --target-port=80 --name=myapp-service
    kubectl get services
    ```

6. **Test the Service by accessing it:**

    Use `kubectl port-forward` to local access:

    ```bash
    kubectl port-forward svc/myapp-service 8080:80
    ```

    Access the service in a web browser or using curl:

    ```bash
    curl http://localhost:8080
    ```

7. **Switch Service to "green":**

    ```bash
    kubectl patch svc myapp-service -p '{"spec":{"selector":{"app":"myapp","version":"green"}}}'
    ```

8. **Test the updated Service:**

    Access the service again:

    ```bash
    curl http://localhost:8080
    ```
