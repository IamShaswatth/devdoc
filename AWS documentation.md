# AWS Learning Journey - EC2 Instance Setup & Deployment

## Learning Objectives

- Understand AWS EC2 (Elastic Compute Cloud) fundamentals
- Learn how to create and configure EC2 instances
- Deploy both Ubuntu and Windows instances
- Host a sample application on EC2
- Gain hands-on experience with AWS cloud infrastructure

---

## What I Learned

This documentation covers my hands-on experience learning AWS EC2 services, from creating my first instance to deploying a sample application. Each step includes the actual process I followed, challenges I encountered, and solutions I implemented.

---

## Table of Contents

1. [Setting Up Ubuntu Instance](#1-setting-up-ubuntu-instance)
2. [Setting Up Windows Instance](#2-setting-up-windows-instance)
3. [Hosting Application on EC2](#3-hosting-application-on-ec2)
4. [Key Takeaways](#key-takeaways)

---

## 1. Setting Up Ubuntu Instance

### What is EC2?
Amazon EC2 (Elastic Compute Cloud) provides scalable computing capacity in the AWS cloud. It allows you to launch virtual servers (instances) and configure them based on your needs.

### Steps I Followed

**Step 1: Launched Ubuntu Instance**

I started by navigating to the EC2 dashboard and selecting "Launch Instance". Here's what the Ubuntu instance configuration looked like:

![Ubuntu Instance](SCREENSHOTS/ubuntu%20instance.png)

#### Configuration Details:
- **Instance Type**: Selected based on workload requirements (I used t2.micro for learning)
- **AMI**: Ubuntu Server (latest LTS version)
- **Storage**: Configured root volume size
- **Security Group**: Opened necessary ports (SSH - port 22, HTTP - port 80, HTTPS - port 443)
- **Key Pair**: Created and downloaded .pem file for SSH access

**Important Notes:**
- Keep your .pem file secure - it's your access key to the instance
- Make sure to configure security groups properly to allow access
- Remember to start with a free-tier eligible instance for practice

---

## 2. Setting Up Windows Instance

### Why Windows Instance?

Windows instances are useful when you need to run Windows-specific applications, .NET framework applications, or need a Windows Server environment.

### Steps I Followed

**Step 1: Instance Selection**

I created a Windows Server instance following a similar process to Ubuntu but with different configurations:

![Windows Selection](SCREENSHOTS/windows.png)

**Step 2: Windows Instance Configuration**

After launching, here's how my Windows instance looked in the EC2 dashboard:

![Windows Instance](SCREENSHOTS/windows%20instance.png)

#### Key Differences from Ubuntu:
- **AMI**: Windows Server (various versions available)
- **RDP Access**: Port 3389 enabled in security group instead of SSH
- **Password Authentication**: Retrieved Windows password using the key pair
- **Licensing**: Windows instances have additional licensing costs

**Connection Process:**
1. Selected the instance and clicked "Connect"
2. Downloaded RDP file
3. Retrieved password using the .pem key pair
4. Connected via Remote Desktop Protocol (RDP)

---

## 3. Hosting Application on EC2

### Deploying a Sample Application

After setting up the instances, I deployed a sample application to understand the hosting process on EC2.

![Sample Hosting on EC2](SCREENSHOTS/sample%20hosting%20in%20ec2.png)

### Steps I Followed:

#### For Ubuntu (Web Application):
1. **Connected via SSH**
   ```bash
   ssh -i "your-key.pem" ubuntu@your-instance-public-ip
   ```

2. **Updated System Packages**
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

3. **Installed Web Server (Apache/Nginx)**
   ```bash
   sudo apt install apache2 -y
   # or
   sudo apt install nginx -y
   ```

4. **Deployed Application Files**
   - Uploaded application files to `/var/www/html/`
   - Configured necessary permissions
   - Set up any required databases or dependencies

5. **Configured Security Groups**
   - Ensured HTTP (80) and HTTPS (443) ports were open
   - Tested access via public IP address

#### For Windows (Web Application):
1. **Connected via RDP**
2. **Installed IIS (Internet Information Services)**
3. **Deployed application to IIS directory**
4. **Configured firewall rules**
5. **Tested access via public IP address

### Challenges I Faced:
- **Security Groups**: Initially forgot to open HTTP port, couldn't access the application
- **File Permissions**: Had to set proper permissions on Ubuntu for web server access
- **Elastic IP**: Public IP changed on restart until I allocated an Elastic IP

---

### Best Practices I Learned:
1. **Always tag your resources** - Makes management much easier
2. **Use Elastic IPs** - For production applications that need consistent IP addresses
3. **Regular snapshots** - Create AMI backups of configured instances
4. **Monitor costs** - Keep an eye on the billing dashboard, especially for running instances
5. **Security first** - Never leave unnecessary ports open, use key pairs securely


---


---


---


---


