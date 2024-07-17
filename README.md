# Deploying Jenkins on Amazon EC2

This project demonstrates the steps to deploy Jenkins on an Amazon EC2 instance. Jenkins is a popular open-source automation server used to automate tasks related to building, testing, and deploying software.

## Prerequisites

Before starting, ensure you have the following:

1. An AWS account
2. Basic knowledge of AWS EC2
3. Git Bash or any SSH client installed on your local machine

## Steps

### 1. Create an EC2 Instance

- **Instance Type:** Choose an instance type suitable for your workload.
- **Key Pair:** Download the key pair (.pem file) for SSH access.

### 2. Configure Security Group

- **SSH Access:** Enable SSH access from your IP address (default port 22).
- **Jenkins Port:** Open port 8080 for Jenkins.

### 3. User Data Script

This script automates the entire setup process of Jenkins, ensuring that all necessary packages are installed and configured without requiring manual intervention. By using this script in the user data field when launching an EC2 instance, Jenkins is ready to use as soon as the instance is up and running.
Add the following user data script during the instance creation to automate the Jenkins setup:

```bash
#! /bin/bash

sudo apt update
sudo apt install openjdk-11-jdk -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
```
Script Explanation:
- Shebang (#!/bin/bash): Specifies the script interpreter as bash.
- Update Package Lists (sudo apt update): Ensures the latest information on the newest versions of packages and their dependencies is available.
- Install OpenJDK 11 (sudo apt install openjdk-11-jdk -y): Installs Java Development Kit 11, which is required to run Jenkins, with automatic confirmation.
- Download Jenkins Key (curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null): Downloads and adds the Jenkins package signing key to the system’s keyring for package verification.
- Add Jenkins Repository (echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null): Adds the Jenkins repository to the system's software sources list to allow Jenkins installation using the package manager.
- Update Package Lists Again (sudo apt-get update): Refreshes the package lists to include the newly added Jenkins repository.
- Install Jenkins (sudo apt-get install jenkins -y): Installs Jenkins with automatic confirmation.

This script has been created based on Jenkins documentation.

### 4. Connect to Your Instance

Once the instance is running, connect to it using SSH:

```bash
ssh -i <key-pair-path> ubuntu@<public-ip-of-instance>
```

### 5. Verify Jenkins Service

Check if Jenkins is running with the following command:

```bash
systemctl status jenkins
```

### 6. Access Jenkins

1. Retrieve the default admin password:

    ```bash
    cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

2. Open your browser and navigate to `http://<public-ip-of-instance>:8080`.
3. Enter the default admin password obtained from the previous step.
4. Complete the initial Jenkins setup by installing the suggested plugins.

### 7. Jenkins Setup

After completing the setup, you will be greeted with the Jenkins welcome page. Jenkins is now successfully deployed on your EC2 instance.

## Screenshots

Insert screenshots here to illustrate the steps:

1. **Security Group Configuration:**
   
   <img width="1208" alt="image" src="https://github.com/user-attachments/assets/554d24d0-6f79-4404-be0b-9a1811fb03eb">
   
3. **Log in to the EC2 instance from Git Bash:**
   
    <img width="483" alt="image" src="https://github.com/user-attachments/assets/9aa7fc73-67c7-4ea2-84ad-eaf615e9a397">
    
5. **Check if the jenkins service is running:**
   
    <img width="635" alt="image" src="https://github.com/user-attachments/assets/6bd282d5-629e-4bcd-961b-27f9d0a3da2b">
    
7. **Jenkins Setup Page:**
   
    <img width="640" alt="image" src="https://github.com/user-attachments/assets/0b2327d9-6df7-4c7d-99c0-a40737a689f4">
    
9. **Welcome to Jenkins:**
    
    <img width="1280" alt="image" src="https://github.com/user-attachments/assets/81d1c184-32f4-4dc0-8edf-3eaeab1fd941">

## Conclusion

This README provided a step-by-step guide to deploy Jenkins on an Amazon EC2 instance. With Jenkins up and running, you can now start automating your software development processes.
