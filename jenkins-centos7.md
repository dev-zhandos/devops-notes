# Jenkins Installation on CentOS 7

This guide provides step-by-step instructions for installing and setting up Jenkins, the popular automation server, on a CentOS 7 server.

## Prerequisites

**System Requirements**

* **Operating System:** CentOS 7
* **Hardware:**
    * **CPU:** 2 cores or more
    * **RAM:** 2 GB or more (4 GB recommended)
    * **Disk Space:** 50 GB or more
* **Java:** OpenJDK 8 or 11 (Jenkins requires Java to run)
* **Web Browser:** A modern web browser for accessing the Jenkins dashboard
* **Network:** Outbound internet access for plugin installation and updates

**Additional Software**

* `sudo` access on the CentOS 7 server

## Installation Steps

1. **Install Java (if not already installed):**

   ```bash
   sudo yum install java-11-openjdk -y 
   ```

2. **Add Jenkins Repository:**

   ```bash
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
   ```

3. **Install Jenkins:**

   ```bash
   sudo yum install jenkins -y
   ```

4. **Start Jenkins Service:**

   ```bash
   sudo systemctl start jenkins
   ```

5. **Enable Jenkins Service on Boot:**

   ```bash
   sudo systemctl enable jenkins
   ```

6. **Adjust Firewall (if enabled):**
   ```bash
   sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
   sudo firewall-cmd --reload
   ```
   (Note: If you're using a different firewall, adjust the commands accordingly.)

7. **Access Jenkins:**

   Open a web browser and go to `http://your_server_ip_or_domain:8080`. You'll be prompted to enter an initial administrative password, which can be found in:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

## Post-Installation Setup

* Follow the on-screen instructions to complete the Jenkins setup wizard. This includes:
    * Installing suggested plugins or selecting custom plugins.
    * Creating your first admin user.

## Additional Notes

* **Security:** It's crucial to secure your Jenkins instance properly. Refer to the Jenkins documentation for security best practices.
* **Reverse Proxy (optional):** Consider setting up a reverse proxy (like Nginx or Apache) to provide a more user-friendly URL and secure access.