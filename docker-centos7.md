# Docker Installation on CentOS 7

This guide will walk you through the steps required to install and set up Docker on your CentOS 7 server.

## Prerequisites

* A CentOS 7 server with sudo privileges
* An internet connection

## Steps

1. **Update System Packages:**
   ```bash
   sudo yum update -y
   ```

2. **Install Required Utilities:**
   ```bash
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

3. **Add Docker Repository:**
   ```bash
   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   ```

4. **Install Docker Engine:**
   ```bash
   sudo yum install -y docker-ce docker-ce-cli containerd.io
   ```

5. **Start Docker Service:**
   ```bash
   sudo systemctl start docker
   ```

6. **Enable Docker on Boot:**
   ```bash
   sudo systemctl enable docker
   ```

7. **Verify Docker Installation:**
   ```bash
   sudo docker run hello-world
   ```
   You should see a "Hello from Docker!" message indicating a successful installation.

## Post-Installation (Optional)

* **Add User to Docker Group (for non-root usage):**
   ```bash
   sudo usermod -aG docker <your_username>
   ```
   Log out and log back in for the changes to take effect.

* **Configure Docker Daemon (optional):**
   Edit `/etc/docker/daemon.json` for custom settings like proxy, storage driver, etc. (Create it if it doesn't exist).

* **Install Docker Compose (optional):**
   For managing multi-container Docker applications.

## Additional Notes

* **Older Docker Versions:**  If you need an older version, search the repository for specific packages.
* **Firewall:** If you have a firewall, open ports (e.g., 2375/tcp, 2376/tcp) as needed. 
* **Official Documentation:** For the latest and most comprehensive instructions, refer to the official Docker documentation: [https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)
