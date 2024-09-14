# Kubernetes Architecture and Networking Challenges
This repository provides a comprehensive overview of Kubernetes architecture, detailing the roles of its control plane and worker nodes. It delves into the intricacies of Kubernetes' core components, explaining how they interact to manage containerized applications effectively. Additionally, this documentation explores the complex networking challenges that arise in Kubernetes environments and the solutions implemented to address them.

Through detailed explanations and illustrations, this guide aims to demystify Kubernetes for both beginners and seasoned practitioners, offering valuable insights into its inner workings and best practices for cluster management.

# Table of Contents
- [Kubernetes Architecture](#kubernetes-architecture)
  - [Control Plane Node Components](#control-plane-node-components)
  - [Importance of the Control Plane](#importance-of-the-control-plane)
  - [Persisting Cluster State](#persisting-cluster-state)
- [Worker Node Components](#worker-node-components)
  - [Container Runtime](#container-runtime)
  - [Node Agent - kubelet](#node-agent---kubelet)
  - [kubelet - CRI Shims](#kubelet---cri-shims)
  - [Proxy - kube-proxy](#proxy---kube-proxy)
  - [Add-ons](#add-ons)
- [Networking Challenges in Kubernetes](#networking-challenges-in-kubernetes)

# Kubernetes Architecture

## At a very high level, Kubernetes is a cluster of compute systems categorized by their distinct roles:
![bpcsp24duwc1-TrainingImage](https://github.com/user-attachments/assets/86fbd27a-e3a0-4c32-ad5a-06854e6c8a14)

- One or more control plane nodes
- One or more worker nodes (optional, but recommended).


# Kubernetes Control Plane Node Overview
![image](https://github.com/user-attachments/assets/7b2b1e9e-4b48-470b-922e-f7f1db7ed9ae)


The control plane node provides an environment for the control plane agents, which are responsible for managing the state of a Kubernetes cluster. It acts as the "brain" behind all cluster operations. The control plane components each have specific roles in managing the cluster. To interact with the Kubernetes cluster, users can send requests to the control plane using:

- **Command Line Interface (CLI) Tool**
- **Web User-Interface (Web UI) Dashboard**
- **Application Programming Interface (API)**

## Importance of the Control Plane

Maintaining the control plane's availability is critical. If the control plane goes down, it can result in cluster downtime, potentially disrupting services and causing business losses. To prevent this, control plane node replicas can be added and configured in High-Availability (HA) mode. 

- **Active Management:** Only one control plane node actively manages the cluster at any given time.
- **Synchronized Components:** All control plane components stay in sync across the control plane node replicas.
- **Enhanced Resiliency:** This HA configuration ensures resiliency in case the active control plane node fails.

## Persisting Cluster State

The cluster's state is stored in a distributed key-value store that contains only cluster state-related data, not client workload data. This key-value store can be configured in two ways:

### 1. Stacked Topology

- The key-value store runs on the control plane node.
- In HA mode, the control plane node replicas also ensure the resiliency of the key-value store.

### 2. External Topology

- The key-value store runs on a dedicated host, separate from the control plane agents.
- This reduces the risk of data loss by decoupling it from the control plane.
- In this setup, the key-value store hosts must be independently replicated for HA, which increases hardware and operational costs.


# Control Plane Node Components
![tibhjua9vb2g-StackedetcdTopology](https://github.com/user-attachments/assets/ba69acc0-7775-47e0-b8ff-1ba5fb814521)



A control plane node runs the following essential control plane components and agents:
- **API Server**
- **Scheduler**
- **Controller Managers**
- **Key-Value Data Store**

Additionally, the control plane node includes:
- **Container Runtime**
- **Node Agent (kubelet)**
- **Proxy (kube-proxy)**
- Optional add-ons for observability, such as a dashboard, cluster-level monitoring, and logging.

## Components Overview

### 1. API Server

The `kube-apiserver` is the central control plane component coordinating all administrative tasks. It handles RESTful calls from users, administrators, developers, operators, and external agents. The API Server reads the current state of the Kubernetes cluster from the key-value store, processes requests, and updates the cluster state in the key-value store for persistence.

- **Key Functions:** 
  - Acts as the only control plane component communicating directly with the key-value store.
  - Serves as an intermediary for other control plane agents that need cluster state information.
- **Scalability:** 
  - Highly configurable and supports horizontal scaling.
  - Can act as a proxy to custom secondary API servers for routing incoming RESTful calls based on defined rules.

### 2. Scheduler

The `kube-scheduler` assigns new workload objects, like pods, to nodes (usually worker nodes) based on the cluster's current state and the workload's requirements.

- **Process:** 
  - Retrieves resource usage data from the key-value store via the API Server.
  - Considers various factors such as Quality of Service (QoS), data locality, node affinity/anti-affinity, taints, tolerations, and cluster topology.
  - Filters potential nodes with predicates, scores them with priorities, and selects the best candidate for hosting the new workload.
- **Configurable:** 
  - Supports custom schedulers and configuration policies. If a workload specifies a custom scheduler, the default scheduler is bypassed.

### 3. Controller Managers

Controller managers run processes to regulate the cluster's state. They constantly compare the cluster's desired state (from configuration data) with its current state (obtained via the API Server) and take corrective actions if there is a mismatch.

- **Types of Controllers:**
  - **`kube-controller-manager`:** Manages nodes' availability, pod counts, endpoints, service accounts, and API access tokens.
  - **`cloud-controller-manager`:** Interacts with cloud providers to manage storage volumes, load balancing, routing, and node availability.

### 4. Key-Value Data Store

`etcd` is an open-source, strongly consistent, distributed key-value store used to persist the Kubernetes cluster's state. Only the API Server communicates with `etcd` to read or write cluster state information.

- **Key Features:**
  - **Append-Only:** New data is appended; existing data is never replaced.
  - **Compaction:** Periodic shredding of obsolete data to reduce the data store's size.
- **Management:** 
  - Comes with a CLI tool (`etcdctl`) that allows snapshot save and restore capabilities, useful for single-instance clusters in development environments.
- **High Availability:** 
  - In production, `etcd` should be replicated in HA mode for data resiliency.
  - Some bootstrapping tools like `kubeadm` provision stacked `etcd` nodes by default, where `etcd` runs alongside other control plane components.

# Worker Node Components
![1_Sk70Ha3JzluR9ZRgcN3PnA](https://github.com/user-attachments/assets/f9409e45-05c1-46c7-8644-b371db69cb8b)

A worker node includes the following components:
- **Container Runtime**
- **Node Agent (kubelet)**
- **kubelet - CRI Shims**
- **Proxy (kube-proxy)**
- **Add-ons:** DNS, observability tools (dashboards, monitoring, logging), and device plugins.

## Components Overview

### 1. Container Runtime

Kubernetes, as a container orchestration engine, relies on a container runtime to handle and run containers. Each node (both control plane and worker) requires a runtime to manage the lifecycle of containers. Control plane components are often run as containers, making the runtime essential even on control plane nodes.

Supported container runtimes include:
- **CRI-O:** A lightweight runtime for Kubernetes that supports quay.io and Docker Hub image registries.
- **containerd:** A simple, robust, and portable container runtime.
- **Docker Engine:** A widely-used platform that employs `containerd` as its runtime.
- **Mirantis Container Runtime:** Previously known as Docker Enterprise Edition.

### 2. Node Agent - kubelet

The `kubelet` is an agent that runs on each node (both control plane and worker) and communicates with the control plane. It receives pod definitions, primarily from the API Server, and interacts with the container runtime to run the containers. Additionally, it monitors the health and resource usage of running pods.

- **Container Runtime Interface (CRI):** 
  - The `kubelet` connects to container runtimes through the Container Runtime Interface (CRI), which includes protocol buffers, gRPC API, libraries, and tools.
  - A CRI shim acts as an abstraction layer, allowing `kubelet` to connect with different container runtimes.
![w7gafyewbmxj-ContainerRuntimeInterface2023](https://github.com/user-attachments/assets/910e8a13-cf27-4d77-b4b1-ae1668933e1f)

- **CRI Services:**
  - **ImageService:** Handles image-related operations.
  - **RuntimeService:** Manages pod and container-related operations.

### 3. kubelet - CRI Shims

Initially, `kubelet` supported only a couple of container runtimes, such as Docker Engine and `rkt`, integrated directly into its source code. However, Kubernetes has since moved to a more flexible, standardized method of integrating container runtimes through the CRI.

- **Shims:** CRI shims are implementations or adapters specific to each supported container runtime.
  - **cri-containerd:** Allows containers to be managed with `containerd` at `kubelet`'s request.
  - **CRI-O:** Enables the use of any Open Container Initiative (OCI) compatible runtime with Kubernetes.
  - **dockershim and cri-dockerd:** Previously, `dockershim` enabled Kubernetes to use the Docker Engine as a runtime. However, starting with Kubernetes v1.24, `dockershim` is deprecated. `cri-dockerd` is now maintained to ensure Docker Engine's continued compatibility with Kubernetes.
![8a4wetvbf38u-dockershimtocri-dockerd](https://github.com/user-attachments/assets/e8c7bf5c-b202-4280-af69-c4dbee74577a)

### 4. Proxy - kube-proxy
![kubernetes_architecture](https://github.com/user-attachments/assets/b1b51786-d779-4976-b1b1-4535a4c0ff9f)

The `kube-proxy` is a network agent running on each node, managing dynamic updates and maintenance of networking rules. It abstracts pod networking and forwards connection requests to the appropriate containers within the pods.

- **Responsibilities:**
  - Manages TCP, UDP, and SCTP stream forwarding.
  - Implements forwarding rules defined through Service API objects.

- **Integration with `iptables`:**
  - Works in conjunction with `iptables`, a firewall utility on Linux, to manage network traffic.

### 5. Add-ons

Add-ons provide additional cluster features not available by default in Kubernetes. These include:

- **DNS:** A DNS server that assigns DNS records to Kubernetes objects and resources.
- **Dashboard:** A web-based UI for cluster management.
- **Monitoring:** Collects cluster-level container metrics and stores them centrally.
- **Logging:** Gathers cluster-level container logs and saves them for analysis.
- **Device Plugins:** Advertises hardware resources (e.g., GPU, FPGA) to application pods.

---

This overview of worker node components highlights the importance of the container runtime, the role of the `kubelet`, network management with `kube-proxy`, and additional functionality provided by add-ons.


# Networking Challenges in Kubernetes

Decoupled, microservices-based applications rely heavily on networking to simulate the tight coupling found in monolithic applications. Networking can be complex to understand and implement, and Kubernetes is no exception. As a microservices orchestrator, Kubernetes addresses several key networking challenges:

1. **Container-to-Container communication inside Pods**
2. **Pod-to-Pod communication on the same node and across cluster nodes**
3. **Service-to-Pod communication within the same namespace and across namespaces**
4. **External-to-Service communication for client access to applications in a cluster**

These challenges must be resolved using Kubernetes and its plugins.

## Networking Challenges Breakdown

### 1. Container-to-Container Communication inside Pods

When a container runtime starts a container, it creates an isolated network space called a **network namespace** using the host operating system's kernel virtualization features. On Linux, this network namespace can be shared between containers or with the host OS.

- In Kubernetes, when a Pod (a group of containers) is created, a **Pause container** is initialized. This container creates a network namespace that all other containers within the Pod share.
- By sharing the network namespace of the Pause container, the containers in the Pod can communicate with each other using `localhost`.

### 2. Pod-to-Pod Communication Across Nodes

Kubernetes allows Pods to be scheduled on nodes in an unpredictable manner. However, every Pod must be able to communicate with any other Pod in the cluster without using Network Address Translation (NAT). This is a fundamental requirement of Kubernetes networking.

- The Kubernetes networking model, often called **"IP-per-Pod"**, gives each Pod a unique IP address, similar to virtual machines (VMs) on a network.
- Inside each Pod, containers share the Pod's network namespace and coordinate port assignments, enabling communication via `localhost`.
- **Container Network Interface (CNI):** Kubernetes uses CNI plugins to integrate containers into the overall network model. CNI is a set of specifications and libraries that allow plugins to configure container networking.
  - The container runtime delegates IP assignment to CNI, which then connects to a plugin (e.g., Bridge or MACvlan) to acquire an IP address.
  - Some popular CNI plugins include Flannel, Weave, Calico, and Cilium. These plugins implement the Kubernetes networking model and often provide support for network policies.

![83mpgq3bdu09-ContainerNetworkInterfaceCNICorePlugins2023](https://github.com/user-attachments/assets/c7de73e3-f8a7-406f-b0ee-3a4da2981f38)

For more detailed information, refer to the [Kubernetes documentation](https://kubernetes.io/docs/).

### 3. External-to-Pod Communication

To allow external access to applications running in Pods, Kubernetes uses **Services**. These services encapsulate network routing rules, which are stored in `iptables` on cluster nodes and managed by `kube-proxy` agents.

- By exposing services using `kube-proxy`, Kubernetes makes the application accessible outside the cluster via a virtual IP address and a dedicated port.



