# Jenkins Installation on Ubuntu

This guide provides step-by-step instructions for installing and setting up Jenkins, the popular automation server, on an Ubuntu server.

## Prerequisites

**System Requirements**

* **Operating System:** Ubuntu 18.04 LTS or later (20.04 LTS recommended)
* **Hardware:**
    * **CPU:** 2 cores or more
    * **RAM:** 2 GB or more (4 GB recommended)
    * **Disk Space:** 50 GB or more
* **Java:** OpenJDK 8 or 11 (Jenkins requires Java to run)
* **Web Browser:** A modern web browser for accessing the Jenkins dashboard
* **Network:** Outbound internet access for plugin installation and updates

**Additional Software**

* `sudo` access on the Ubuntu server

## Installation Steps

1. **Install Java (if not already installed):**

   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk -y
   ```

2. **Add Jenkins Repository Key:**

   ```bash
   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
   ```

3. **Add Jenkins Repository:**

   ```bash
   sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   ```

4. **Update Package Lists:**

   ```bash
   sudo apt update
   ```

5. **Install Jenkins:**

   ```bash
   sudo apt install jenkins -y
   ```

6. **Start Jenkins Service:**

   ```bash
   sudo systemctl start jenkins
   ```

7. **Enable Jenkins Service:**

   ```bash
   sudo systemctl enable jenkins
   ```

8. **Access Jenkins:**

   Open a web browser and go to `http://your_server_ip_or_domain:8080`. You'll be prompted to enter an initial administrative password, which can be found in:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

## Post-Installation Setup

* Follow the on-screen instructions to complete the Jenkins setup wizard. This includes:
    * Installing suggested plugins or selecting custom plugins.
    * Creating your first admin user.

## Additional Notes

* **Firewall:** If you have a firewall enabled, open port 8080 (or the custom port you configured) to allow incoming connections.
* **Security:** It's crucial to secure your Jenkins instance properly. Refer to the Jenkins documentation for security best practices.
* **Reverse Proxy (optional):** Consider setting up a reverse proxy (like Nginx or Apache) to provide a more user-friendly URL and secure access.
