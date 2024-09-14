# Kubernetes Architecture

## At a very high level, Kubernetes is a cluster of compute systems categorized by their distinct roles:

- One or more control plane nodes
- One or more worker nodes (optional, but recommended).
![bpcsp24duwc1-TrainingImage](https://github.com/user-attachments/assets/86fbd27a-e3a0-4c32-ad5a-06854e6c8a14)


# Kubernetes Control Plane Node Overview
![image](https://github.com/user-attachments/assets/8bd02207-fa0e-4017-b1e7-31d3be07ee93)

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

