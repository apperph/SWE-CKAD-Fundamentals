# Lab 4: Create CronJobs and Jobs

#### Exercise 1: Creating a Simple Job

**Instructions:**

1. Create a YAML file named `job-simple.yaml` with the following content:

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: job-simple
    spec:
      template:
        spec:
          containers:
          - name: simple-job
            image: busybox
            command: ["echo", "Hello, Kubernetes!"]
          restartPolicy: Never
    ```

2. Apply the manifest to create the Job:

    ```bash
    kubectl apply -f job-simple.yaml
    kubectl get jobs
    kubectl get pods
    ```

### 2. Job with Parallelism

#### Exercise 2: Parallel Jobs

**Instructions:**

1. Create a YAML file named `job-parallel.yaml` with parallelism:

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: job-parallel
    spec:
      parallelism: 3
      completions: 3
      template:
        spec:
          containers:
          - name: parallel-job
            image: busybox
            command: ["echo", "Running parallel job"]
          restartPolicy: Never
    ```

2. Apply the manifest:

    ```bash
    kubectl apply -f job-parallel.yaml
    kubectl get jobs
    kubectl get pods
    ```

### 3. CronJobs

#### Exercise 3: Creating a Simple CronJob

**Instructions:**

1. Create a YAML file named `cronjob-simple.yaml` with the following content:

    ```yaml
    apiVersion: batch/v1
    kind: CronJob
    metadata:
      name: cronjob-simple
    spec:
      schedule: "*/1 * * * *" # Runs every minute
      jobTemplate:
        spec:
          template:
            spec:
              containers:
              - name: simple-cronjob
                image: busybox
                command: ["echo", "Hello from the CronJob"]
              restartPolicy: Never
    ```

2. Apply the manifest to create the CronJob:

    ```bash
    kubectl apply -f cronjob-simple.yaml
    kubectl get cronjobs
    ```
#### Exercise 4: Create a Pi Calculation Job

1. Create a Job named `pi` with `perl` image:

    ```bash
    kubectl create job pi --image=perl:5.34 -- perl -Mbignum=bpi -wle 'print bpi(2000)'
    ```

2. Wait till it's done and get the output:

    ```bash
    kubectl get jobs -w # wait till 'SUCCESSFUL' is 1 (will take some time, perl image might be big)
    kubectl get po # get the pod name
    kubectl logs pi-**** # get the pi numbers
    ```

    OR

    ```bash
    kubectl get jobs -w # wait till 'SUCCESSFUL' is 1 (will take some time, perl image might be big)
    kubectl logs job/pi
    ```

    OR

    ```bash
    kubectl wait --for=condition=complete --timeout=300s job pi
    kubectl logs job/pi
    ```

