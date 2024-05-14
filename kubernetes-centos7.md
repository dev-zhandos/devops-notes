# Kubernetes Installation on CentOS 7 (Master and Worker Nodes)

This guide walks you through the process of setting up a Kubernetes cluster on CentOS 7, consisting of a master node and one or more worker nodes.

## Prerequisites

### System Requirements (for all nodes)

* **Operating System:** CentOS 7 (64-bit)
* **Hardware:**
    * **CPU:**  2 cores or more (recommended 2+ cores per node)
    * **RAM:**  2 GB minimum (recommended 4GB+ per node)
    * **Disk Space:**  20 GB minimum (varies depending on workload)
* **Network:**
    * **Connectivity:** Stable network connection with outbound internet access.
    * **Port Access:** Ensure the following ports are open (or adjust firewall rules accordingly):
        * 6443 (Kubernetes API Server)
        * 2379-2380 (etcd server client API)
        * 10250 (kubelet API)
        * 10259 (kube-scheduler)
        * 10257 (kube-controller-manager)
* **Other:**
    * **SSH Access:** You'll need SSH access to all servers for installation and configuration.
    * **Sudo Privileges:** Sudo or root privileges are required on each server.
    * **Unique Hostnames:** Ensure each node has a unique hostname.
    * **SELinux:** It's recommended to disable SELinux or configure it for Kubernetes if it's enabled.

* Two or more CentOS 7 servers (one for the master, others for workers)
* SSH access to each server
* Sudo privileges on each server
* Adequate system resources (RAM, CPU)
* Basic understanding of Linux commands and networking

## Steps

### 1. Preparation (All Nodes)

1. **Disable SELinux:**
   ```bash
   sudo setenforce 0
   sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
   ```

2. **Disable Swap:**
   ```bash
   sudo swapoff -a
   sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
   ```

3. **Install Dependencies:**
   ```bash
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

4. **Add Kubernetes Repository:**
   ```bash
   sudo yum-config-manager --add-repo https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
   sudo yum install -y epel-release
   ```

5. **Install Kubernetes Components:**
   ```bash
   sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
   sudo systemctl enable --now kubelet
   ```

6. **Install Container Runtime (containerd):**
   ```bash
   sudo yum install -y containerd.io
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
