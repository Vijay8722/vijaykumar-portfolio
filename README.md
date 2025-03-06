# Deploying a Web Application on AWS EC2 (Ubuntu)

## Overview
This guide explains how to deploy a portfolio website on an **Ubuntu EC2 instance** using **Apache**.

## Prerequisites
- **AWS Account** with access to launch EC2 instances.
- **SSH Key Pair** (.pem file) for connecting to the instance.
- **GitHub repository** containing the portfolio website (ZIP file link required).

---

## 1. Launch an EC2 Instance
### **Steps:**
1. Go to **AWS Console** → Navigate to **EC2** → Click **Launch Instance**.
2. Choose an **Ubuntu AMI** (e.g., Ubuntu 22.04 LTS).
3. Select an instance type (**t2.micro** for free-tier users).
4. Configure storage and security groups:
   - Allow **SSH (port 22)** and **HTTP (port 80)** inbound traffic.
5. Create or use an existing **Key Pair** (.pem file) and download it.
6. Click **Launch Instance**.

---

## 2. Connect to the EC2 Instance
There are multiple ways to connect:

### **Option 1: AWS Console (Recommended)**
- Go to **EC2 Dashboard** → Select the instance → Click **Connect** → Use **EC2 Instance Connect**.

### **Option 2: Using SSH from Local Machine**
```sh
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

### **Option 3: Using PuTTY (Windows Users)**
- Convert `.pem` to `.ppk` using PuTTYgen.
- Open PuTTY → Enter `your-ec2-public-ip` → Load the `.ppk` file.
- Click **Open**.

---

## 3. Update and Install Required Packages
Run the following commands after logging in:
```sh
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y apache2 unzip wget
```

---

## 4. Start and Enable Apache Web Server
```sh
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2
```
Ensure that Apache is running before proceeding.

---

## 5. Download and Deploy Portfolio Website
1. Create a directory for the project:
   ```sh
   mkdir ~/aws_project && cd ~/aws_project
   ```
2. Download the portfolio ZIP file from GitHub:
   ```sh
   wget https://github.com/your-repo/portfolio.zip
   ```
3. Extract the ZIP file:
   ```sh
   unzip portfolio.zip
   ```
4. Move website files to Apache’s root directory:
   ```sh
   sudo mv * /var/www/html/
   ```

---

## 6. Configure Firewall (If Necessary)
Enable HTTP traffic:
```sh
sudo ufw allow 'Apache Full'
sudo ufw enable
```

---

## 7. Verify Deployment
1. Restart Apache:
   ```sh
   sudo systemctl restart apache2
   ```
2. Copy the **Public IPv4 Address** of your instance.
3. Open it in a web browser:  
   ```
   http://your-ec2-public-ip
   ```
4. Your portfolio website should now be live! 🎉

---

## Conclusion
You have successfully deployed a portfolio website on an Ubuntu EC2 instance using Apache. 🚀

For any issues, check Apache logs:
```sh
sudo journalctl -xe
```
