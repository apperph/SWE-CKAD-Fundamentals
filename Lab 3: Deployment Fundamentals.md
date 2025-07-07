# Lab 3: Deployment Fundamentals

#### Exercise 1: Creating a Basic Deployment


1. Create a YAML file named `deploy-nginx.yaml` with the following content:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: deploy-nginx
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:latest
    ```

2. Apply the manifest to create the Deployment:

    ```bash
    kubectl apply -f deploy-nginx.yaml
    kubectl get deployments
    kubectl get pods
    ```

#### Exercise 2: Updating the Deployment Image

**Instructions:**

1. Update the Deployment to use a specific Nginx version:

    ```bash
    kubectl set image deployment/deploy-nginx nginx=nginx:1.19 --record
    kubectl rollout status deployment/deploy-nginx
    kubectl describe deployment deploy-nginx
    ```

2. Verify that the update was successful:

    ```bash
    kubectl get pods
    kubectl get deployment deploy-nginx -o yaml
    ```

#### Exercise 3: Scaling the Deployment

**Instructions:**

1. Scale the Deployment to 6 replicas:

    ```bash
    kubectl scale deployment deploy-nginx --replicas=6
    kubectl get pods
    ```

2. Scale down to 2 replicas and observe:

    ```bash
    kubectl scale deployment deploy-nginx --replicas=2
    kubectl get pods
    ```

#### Exercise 4: Rolling Back Updates

**Instructions:**

1. Roll back to the previous revision:

    ```bash
    kubectl rollout undo deployment deploy-nginx
    kubectl rollout status deployment deploy-nginx
    ```

2. Verify the rollback:

    ```bash
    kubectl describe deployment deploy-nginx
    kubectl get pods
    ```


#### Exercise 5: Deploying a Wrong Image

**Instructions:**

1. Update the Deployment with an incorrect image version intentionally:

    ```bash
    kubectl set image deployment/deploy-nginx nginx=nginx:wrongversion --record
    ```

2. Monitor the rollout to observe failure:

    ```bash
    kubectl rollout status deployment deploy-nginx
    ```

3. Check the rollout history to see past revisions:

    ```bash
    kubectl rollout history deployment deploy-nginx
    ```

4. Check the status and details of the pods to understand the failure:

    ```bash
    kubectl get pods
    kubectl describe pods
    kubectl logs <pod-name>
    ```

5. Roll back to the last working version:

    ```bash
    kubectl rollout undo deployment deploy-nginx
    ```

6. Verify that the rollback was successful and check the Deployment status:

    ```bash
    kubectl get deployments
    kubectl describe deployment deploy-nginx
    ```

7. Review the rollout history again to see the rollback:

    ```bash
    kubectl rollout history deployment deploy-nginx
    ```
