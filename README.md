# Jenkins Installation on Amazon EC2 (Ubuntu)

**Continuous Integration and Continuous Deployment (CI/CD)** is a DevOps practice that enables development teams to deliver code changes more frequently and reliably. Jenkins is an open-source automation server that provides hundreds of plugins to support building, deploying, and automating any project.

Benefits of Jenkins in DevOps:

    Automation: Automates the entire software development lifecycle

    Integration: Integrates with various development, testing, and deployment tools

    Extensibility: Large plugin ecosystem for extended functionality

    Distributed builds: Master-agent architecture for scalable builds

    Pipeline as code: Define build pipelines using code (Jenkinsfile)

## Prerequisites

    ✅ An AWS account

    ✅ Basic knowledge of AWS EC2 and SSH

    ✅ A security group with appropriate ports (22 & 8080) open

Step 1: Launch an Ubuntu EC2 Instance

    Log in to your AWS Management Console

    Navigate to EC2 service and click "Launch Instance"

    Configure your instance:

        Name: Jenkins-Server

        AMI: Ubuntu Server 22.04 LTS

        Instance type: t2.medium (recommended for Jenkins with Docker)

        Key pair: Create or select an existing key pair

        Network settings: Create security group with rules below

        Storage: 25 GB GP3 volume

![ec2](./jenk1.png)

    Security Group Configuration:

        SSH (port 22) - Your IP only

        HTTP (port 80) - 0.0.0.0/0

        Custom TCP (port 8080) - 0.0.0.0/0 (Jenkins web interface)

    Launch instance and download your key pair


![sg](./jenk2.png)


Step 2: Connect to Your EC2 Instance

```bash

# Change permissions on your key file
chmod 400 your-key-pair.pem

# Connect to your Ubuntu instance
ssh -i "your-key-pair.pem" ubuntu@your-ec2-public-ip

Step 3: Update System and Install Java
bash

# Update package list
sudo apt update

# Upgrade existing packages
sudo apt upgrade -y

# Install Java (Jenkins requires Java 11 or 17)
sudo apt install openjdk-11-jdk -y

# Verify Java installation
java -version

```

Step 4: Install Jenkins

```bash

# Add Jenkins repository key to the system
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add Jenkins repository to apt sources
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update package list to include Jenkins repository
sudo apt update

# Install Jenkins
sudo apt install jenkins -y

# Start Jenkins service
sudo systemctl start jenkins

# Enable Jenkins to start on boot
sudo systemctl enable jenkins

# Check Jenkins status
sudo systemctl status jenkins

```

![jenkins_running](./jenks3.png)


Step 5: Access Jenkins Web Interface

    Get the initial admin password:

```bash

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

#######################

```

    Open your web browser and navigate to:

```text

http://your-ec2-public-ip:8080

```

    Enter the admin password when prompted

    Follow the setup wizard to:

        Install suggested plugins

        Create an admin user

        Configure your Jenkins instance

![jenkins_running](./jenks4.png)

## Resources

    📚 Jenkins Official Documentation

    📚 AWS EC2 Documentation

    📚 Ubuntu Server Guide

## Support

If you encounter issues:

    Check Jenkins logs: /var/log/jenkins/jenkins.log

    Verify Java installation: java -version

    Confirm Jenkins is running: sudo systemctl status jenkins

    Ensure security group allows traffic on port 8080 (or 80 if configured)