# Kubernetes Deployments

## Overview

Deployment objects provide declarative updates to Pods and ReplicaSets. The DeploymentController, part of the control plane node's controller manager, ensures that the current state matches the desired state of your running containerized application. It allows for seamless application updates and rollbacks through the default RollingUpdate strategy, and directly manages its ReplicaSets for application scaling. An alternative update strategy is the Recreate method, which is less commonly used.
file:![k6u3a80eclnc-DeploymentReplicaSetACreated2023](https://github.com/user-attachments/assets/4d663a7a-8104-4a01-9800-2aa33247ba73)

## Example Deployment Definition

Below is an example of a Deployment object's definition manifest in YAML format:

 ```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - name: nginx
        image: nginx:1.20.2
        ports:
        - containerPort: 80
 ```

### Explanation of Fields

- **apiVersion**: Specifies the API endpoint on the API server you want to connect to. It must match an existing version for the object type.
- **kind**: Specifies the type of object (in this case, `Deployment`).
- **metadata**: Contains basic information about the object, such as its name and labels.
- **spec**: Marks the beginning of the block defining the desired state of the Deployment object. In this example, it requests 3 replicas of the Pod to be running at all times. The Pods are created using the Pod Template defined in `spec.template`.

The Pods created will run a single container using the NGINX image version `1.20.2`.

### Creating a Deployment

To create this Deployment object, you can store the manifest in a file called `def-deploy.yaml` and use the following command:

 ```bash
kubectl create -f def-deploy.yaml
 ```

Advanced users may opt to use `apply` instead:

 ```bash
kubectl apply -f def-deploy.yaml
 ```

### Imperative Deployment Creation

You can also create a Deployment imperatively without using a definition manifest:

 ```bash
kubectl create deployment nginx-deployment \
--image=nginx:1.20.2 --port=80 --replicas=3
 ```

### Generating a Starter Definition Manifest

If you need a starter definition manifest, you can generate one with:

 ```bash
kubectl create deployment nginx-deployment \
--image=nginx:1.20.2 --port=80 --replicas=3 \
--dry-run=client -o yaml > nginx-deploy.yaml
 ```

This command uses the `--dry-run=client` flag to simulate the creation of the Deployment and outputs the YAML to `nginx-deploy.yaml`.

You can also generate a JSON definition file using:

 ```bash
kubectl create deployment nginx-deployment \
--image=nginx:1.20.2 --port=80 --replicas=3 \
--dry-run=client -o json > nginx-deploy.json
 ```


### Loading the Definition Files into the Cluster

Both the YAML and JSON files can be loaded into the cluster using the following commands:

- For the YAML file:
   ```bash
  kubectl create -f nginx-deploy.yaml
   ```

- For the JSON 
   ```bash
  kubectl create -f nginx-deploy.json
   ```

## Deployment Status

Once the Deployment object is created, Kubernetes attaches the status field to it and populates it with necessary information. For example, a new Deployment creates ReplicaSet A, which then creates 3 Pods, each configured to run one instance of the `nginx:1.20.2` container image. This state is recorded as Revision 1.
![g7f3nyhbhcr2-DeploymentReplicaSetBCreated](https://github.com/user-attachments/assets/600b0bac-8907-45ff-8e35-8143fae744d1


## Deployment Operations

Before advancing to more complex topics, familiarize yourself with Deployment operations using the following commands:

1. **Apply changes to a Deployment and record the command:**
    ```bash
   kubectl apply -f nginx-deploy.yaml --record
    ```
   - The `--record` flag records the command in the Deployment's annotation for later reference.

2. **List all Deployments:**
    ```bash
   kubectl get deployments
    ```
   - This command lists all the Deployment objects in the current namespace.

3. **Get detailed information about Deployments:**
    ```bash
   kubectl get deploy -o wide
    ```
   - The `-o wide` option provides additional information such as the number of Pods and their current status.

4. **Scale a Deployment:**
    ```bash
   kubectl scale deploy nginx-deployment --replicas=4
    ```
   - This command adjusts the number of Pod replicas for the specified Deployment to 4.

5. **Get the YAML definition of a specific Deployment:**
    ```bash
   kubectl get deploy nginx-deployment -o yaml
    ```
   - This retrieves the full YAML definition of the specified Deployment.

6. **Get the JSON definition of a specific Deployment:**
    ```bash
   kubectl get deploy nginx-deployment -o json
    ```
   - Similar to the YAML command, this retrieves the Deployment's definition in JSON format.

7. **Describe a Deployment:**
    ```bash
   kubectl describe deploy nginx-deployment
    ```
   - This command provides detailed information about the Deployment, including events and status.

8. **Check the status of a rollout:**
    ```bash
   kubectl rollout status deploy nginx-deployment
    ```
   - This shows the current status of the Deployment's rollout, indicating if it is complete or still in progress.

9. **View rollout history of a Deployment:**
    ```bash
   kubectl rollout history deploy nginx-deployment
    ```
   - This command displays the history of rollouts for the specified Deployment, showing revisions.

10. **View a specific revision's history:**
     ```bash
    kubectl rollout history deploy nginx-deployment --revision=1
     ```
    - Use this command to see details about a specific revision of the Deployment.

11. **Update the image of a Deployment and record the change:**
     ```bash
    kubectl set image deploy nginx-deployment nginx=nginx:1.21.5 --record
     ```
    - This updates the container image for the Deployment and records the command in the history.

12. **View the rollout history for a specific revision:**
     ```bash
    kubectl rollout history deploy nginx-deployment --revision=2
     ```
    - This retrieves details about the second revision of the Deployment.

13. **Undo a Deployment to a previous revision:**
     ```bash
    kubectl rollout undo deploy nginx-deployment --to-revision=1
     ```
    - This rolls back the Deployment to the specified revision.

14. **Get all resources with a specific label:**
     ```bash
    kubectl get all -l app=nginx -o wide
     ```
    - This command lists all resources associated with the label `app=nginx`, providing a wide view.

15. **Delete a Deployment:**
     ```bash
    kubectl delete deploy nginx-deployment
     ```
    - This command removes the specified Deployment and its associated Pods.

16. **Get Deployments, ReplicaSets, and Pods with a specific label:**
     ```bash
    kubectl get deploy,rs,po -l app=nginx
     ```
    - This retrieves all Deployments, ReplicaSets, and Pods with the label `app=nginx`.

