# ☁️ AWS EC2 Web Server Deployment

A hands-on cloud project demonstrating the manual provisioning, configuration, and security setup of a virtual Linux server hosted on AWS EC2.

---

## 🛠️ Tech Stack & Services
* **Cloud Provider:** Amazon Web Services (AWS)
* **Compute Service:** Amazon EC2 (`t3.micro`)
* **Operating System:** Amazon Linux 2023 (AL2023)
* **Package Management Compatibility:** The User Data script utilizes standard `yum` commands (`yum install httpd -y`). While AL2023 natively uses `dnf` as its modern package manager, AL2023 includes a built-in backward-compatibility alias that automatically redirects legacy `yum` calls to `dnf`. This allowed the script to execute seamlessly without breaking dependencies.
* **Web Server:** Apache (HTTPD)
* **Security:** AWS Security Groups (Port 22 SSH, Port 80 HTTP)
  
---

## 📐 Architecture Overview
1. Provisioned an EC2 instance within the default Public Subnet of the default VPC.
2. Configured an Inbound Security Group rule allowing:
   - **HTTP (Port 80)** from anywhere (`0.0.0.0/0`) to serve web traffic.
   - **SSH (Port 22)** restricted for secure remote management.
3. Injected a **User Data script** at launch to automatically automate OS updates, install Apache, enable the system service, and generate a custom landing page.

---

## 📜 User Data Automation Script
#!/bin/bash
# Use this for your user data (script from top to bottom)
# Update system packages & install httpd (Linux 2 version)
yum update -y
# Install Apache HTTP Server
yum install -y httpd
# Start and enable Apache on boot
systemctl start httpd
systemctl enable httpd
# Create custom landing page
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html

# AWS EC2 Automated Deployment

---

## 📌 Architecture & Verification

### 1. EC2 Instance Provisioning
The instance was launched using Amazon Linux 2023 with appropriate tags and instance types.

EC2 Instance Running Console
*Figure 1: AWS Management Console showing the EC2 instance in a healthy Running state.*

<img width="512" height="73" alt="EC2 Instance - Running State" src="https://github.com/user-attachments/assets/be5abf84-c8f8-4ae4-a0fc-2df819cb55c6" />

---

### 2. Security Group Configuration
Inbound rules were configured to allow HTTP traffic on port 80 for web access and SSH traffic on port 22 for administration.

Security Group Inbound Rules
*Figure 2: Security Group inbound rules allowing restricted SSH and HTTP access.*

<img width="512" height="104" alt="Security - Inbound Rules" src="https://github.com/user-attachments/assets/1d3db8ca-3b4f-4ce9-8207-91df3398f10d" />

---

### 3. Web Server Verification
An Apache (`httpd`) web server was automatically configured at launch via User Data. 

Live Web Page Output
*Figure 3: Browser accessing the public IP address displaying the live "Hello World" webpage.*

<img width="512" height="265" alt="Web Server - Linux 2" src="https://github.com/user-attachments/assets/dec04ba1-6828-4c92-9338-d124e17fe7a3" />
