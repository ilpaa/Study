# Kubernetes and `kubectl` Command Reference

## Applying and Creating Resources

1. **Apply a Pod definition from a remote YAML file:**
    ```bash
   kubectl apply -f https://k8s.io/examples/pods/simple-pod.yaml
    ```
   - `apply` is used to apply a configuration to a resource by file or URL. This command fetches a YAML file from the provided URL and applies the resource configuration (in this case, a pod) to your Kubernetes cluster.

2. **Create a Pod from a local YAML file:**
    ```bash
   kubectl create -f def-pod.yaml
    ```
   - `create` is used to create a resource from a file or URL. In this case, it creates a pod based on the definition file `def-pod.yaml`.

## Manually Running a Pod Imperatively

Writing up definition manifests, especially complex ones, may prove to be time-consuming due to YAML's sensitivity to indentation. When editing YAML files, remember that each indent is **two spaces**, and **TAB characters should be avoided**.

If you prefer not to use a YAML definition file, you can run a Pod imperatively with the following command:

 ```bash
kubectl run nginx-pod --image=nginx:1.22.1 --port=80
 ```

- `run` is used to create and start a pod or deployment in an imperative way (i.e., without using a manifest). This command will create a pod named `nginx-pod` with the NGINX image version `1.22.1`, exposing port 80.

## Generating Pod Definition Files

If you need a starter manifest (like a YAML or JSON file), you can generate one without actually creating the resource using the `--dry-run` and `-o yaml` flags.

### Generating a YAML Definition File

 ```bash
kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
--dry-run=client -o yaml > nginx-pod.yaml
 ```

- The `--dry-run=client` flag ensures that the command does not actually create the resource, but only simulates the request on the client side.
- The `-o yaml` flag outputs the result in YAML format, and the result is redirected into a file called `nginx-pod.yaml`.

### Generating a JSON Definition File

You can similarly generate a JSON file by changing the output format:

 ```bash
kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
--dry-run=client -o json > nginx-pod.json
 ```

## Loading the YAML/JSON Files into the Cluster

Once you have generated the YAML or JSON files, you can load them into the cluster using the following commands:

- For the YAML file:
    ```bash
   kubectl create -f nginx-pod.yaml
    ```

- For the JSON file:
    ```bash
   kubectl create -f nginx-pod.json
    ```

## Additional Pod Operations

Here are some basic commands to manage and explore the pod:

- Apply changes to a pod:
    ```bash
   kubectl apply -f nginx-pod.yaml
    ```

- List all pods:
    ```bash
   kubectl get pods
    ```

- Get the YAML definition of a specific pod:
    ```bash
   kubectl get pod nginx-pod -o yaml
    ```

- Get the JSON definition of a specific pod:
    ```bash
   kubectl get pod nginx-pod -o json
    ```

- Describe a pod in detail (status, events, etc.):
    ```bash
   kubectl describe pod nginx-pod
    ```

- Delete a pod:
    ```bash
   kubectl delete pod nginx-pod
    ```
