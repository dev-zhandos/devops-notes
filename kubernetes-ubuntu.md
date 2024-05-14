# Kubernetes Installation on Ubuntu (Master and Worker Nodes)

This guide walks you through the process of setting up a Kubernetes cluster on Ubuntu, consisting of a master node and one or more worker nodes.

## Prerequisites

### System Requirements

* **Operating System:** Ubuntu 20.04 LTS or later (22.04 LTS recommended)
* **Hardware (each node):**
    * **CPU:** 2 cores or more
    * **RAM:** 2 GB or more (4 GB+ recommended)
    * **Disk Space:** 30 GB or more
* **Networking:**
    * Nodes should be able to communicate with each other over a network
    * Open ports: 6443 (Kubernetes API), 2379-2380 (etcd), 10250 (kubelet), 10259 (kube-scheduler), 10257 (kube-controller-manager)
* **Swap:** Disabled (Kubernetes doesn't support swap)

* Two or more Ubuntu servers (one for the master, others for workers)
* SSH access to each server
* Sudo privileges on each server
* Adequate system resources (RAM, CPU)
* Basic understanding of Linux commands and networking

## Steps

### 1. Preparation (All Nodes)

1. **Update Packages:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Disable Swap:**
   ```bash
   sudo swapoff -a
   sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
   ```

3. **Install Dependencies:**
   ```bash
   sudo apt install -y apt-transport-https ca-certificates curl gnupg lsb-release
   ```

4. **Add Kubernetes Repository:**
   ```bash
   sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
   echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
   sudo apt update
   ```

5. **Install Kubernetes Components:**
   ```bash
   sudo apt install -y kubelet kubeadm kubectl
   sudo apt-mark hold kubelet kubeadm kubectl
   ```

6. **Install Container Runtime (containerd):**
   ```bash
   sudo apt install -y containerd.io
   sudo mkdir -p /etc/containerd
   sudo containerd config default | sudo tee /etc/containerd/config.toml
   sudo systemctl restart containerd
   ```

### 2. Master Node Initialization

1. **Initialize Kubernetes Cluster:**
   ```bash
   sudo kubeadm init --pod-network-cidr=10.244.0.0/16
   ```
   * Note the output carefully. It contains the command for joining worker nodes.

2. **Configure kubectl for your User:**
   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

3. **Install Network Add-on (e.g., Calico):**
   ```bash
   kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
   ```

### 3. Join Worker Nodes

1. On **each worker node**, run the `kubeadm join` command provided at the end of the `kubeadm init` output on the master. It will look something like:

   ```bash
   sudo kubeadm join 192.168.X.X:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
   ```

## Verification

On the master node, run:

```bash
kubectl get nodes
```

You should see your master and worker nodes in the `Ready` state.
