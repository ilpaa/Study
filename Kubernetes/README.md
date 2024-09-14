# Kubernetes Architecture

## At a very high level, Kubernetes is a cluster of compute systems categorized by their distinct roles:

- One or more control plane nodes
- One or more worker nodes (optional, but recommended).
![bpcsp24duwc1-TrainingImage](https://github.com/user-attachments/assets/86fbd27a-e3a0-4c32-ad5a-06854e6c8a14)


# Kubernetes Control Plane Node Overview
![tibhjua9vb2g-StackedetcdTopology](https://github.com/user-attachments/assets/ba69acc0-7775-47e0-b8ff-1ba5fb814521)

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
![image](https://github.com/user-attachments/assets/7b2b1e9e-4b48-470b-922e-f7f1db7ed9ae)


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

## Conclusion

Understanding the roles and configuration of each control plane component is crucial for managing the Kubernetes cluster's health and availability. Properly configuring HA modes and utilizing control plane replicas can ensure resilience and fault tolerance within the cluster.


