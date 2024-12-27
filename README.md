# Kubernetes Cluster Setup and Practicals

This guide walks you through setting up a Kubernetes cluster using kubeadm and deploying the sample application **Podinfo**. The cluster consists of one master node and two worker nodes. Then we will further move to do some handson in our cluster. 

---
## Note!
If you want to see all the commands I run on my master cluster then all the history is captured in "logs" folder.
## Prerequisites

1. **System Requirements:**
   - **Host system:** 6-core CPU, 16GB RAM
   - **Hypervisor:** Hyper-V
   - **Guest VMs:**
     - 1 Master Node (e.g., 192.168.100.34)
     - 2 Worker Nodes (e.g., 192.168.100.35, 192.168.100.36)
     - Ubuntu 20.04 installed on all nodes

2. **Network Configuration:**
   - Static IPs assigned to all VMs.
   - Ensure inter-node communication using `ping`.

3. **Required Tools Installed on Each Node:**
   - `openssh-server`
   - `kubeadm`
   - `kubelet`
   - `kubectl`
   - `containerd` (as the container runtime)

4. **Optional:** Install `helm` on the master node (if using Helm charts).

---


