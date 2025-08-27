Hosting VPN Server on AWS Cloud using OpenVPN Protocol

---

**Hosting VPN Server on AWS Cloud using OpenVPN Protocol**

This document provides a detailed step-by-step guide on how to host a VPN server on AWS cloud using the OpenVPN protocol. The process includes setting up an EC2 instance, configuring OpenVPN Access Server, and connecting to the VPN.

---

### **Prerequisites**

- **AWS Account:** You need to have an active AWS account to proceed.
- **SSH Client:** An SSH client like PuTTY or the built-in terminal on Linux/MacOS is required to connect to your EC2 instance.
- **OpenVPN Client:** You will need the OpenVPN client installed on your local machine to connect to the VPN.
- **IAM Permissions:** Ensure that your AWS IAM user has the required permissions to launch EC2 instances, modify security groups, and access the OpenVPN server.

---

### **1. Launch EC2 Instance on AWS**

Follow the steps below to launch your EC2 instance to host the OpenVPN server.

#### **Step 1: Login to AWS Console**
- Open the AWS Management Console.
- Log in with your credentials.

#### **Step 2: Launch EC2 Instance**
- Navigate to EC2 from the AWS Console homepage.
- Click on the **Launch Instance** button to create a new EC2 instance.

#### **Step 3: Choose AMI (Amazon Machine Image)**
- In the **Choose an Amazon Machine Image (AMI)** section, click on **Browse more AMIs**.
- Search for the **OpenVPN Access Server / Self-Hosted VPN (BYOL) AMI** (Bring Your Own License).
- Select the **OpenVPN Access Server** image.

#### **Step 4: Choose Instance Type**
- Select an instance type based on your requirement (e.g., t2.micro for free-tier usage).
- Click **Next: Configure Instance Details**.

#### **Step 5: Configure Instance Details**
- Configure the instance as per your requirements (e.g., VPC, Subnet).
- Click **Next: Add Storage** to proceed.

#### **Step 6: Configure Storage**
- Choose the default storage settings or modify them if needed.
- Click **Next: Add Tags**.

#### **Step 7: Configure Security Group**
- In the **Configure Security Group** section, create a new security group or select an existing one. Add the following rules:
    - **SSH:** Port 22 (for SSH access)
    - **Custom TCP:** Port 443 (for OpenVPN Web Admin)
    - **Custom UDP:** Port 1194 (for OpenVPN traffic)
    - **HTTPS:** Port 443 (for OpenVPN Access Server)
- Click **Review and Launch**.

#### **Step 8: Launch Instance**
- Review the instance configuration.
- Select or create a new Key Pair (e.g., aws.pem).
- Download the `.pem` file and keep it secure.
- Click **Launch** to start the instance.

---

### **2. Connect to the EC2 Instance Using SSH**

#### **Step 1: Prepare the SSH Key**
- Ensure that the SSH private key file (**aws.pem**) is properly secured.
- Run the following command in your terminal to make sure your key is not publicly viewable:
  ```bash
  chmod 400 "aws.pem"
  ```

#### **Step 2: Connect to the Instance**
- Use the following SSH command to connect to the EC2 instance. Replace `ec2-X-XXX-XX-XXX.ap-south-1.compute.amazonaws.com` with your EC2 instance’s Public DNS:
  ```bash
  ssh -i "aws.pem" openvpnas@ec2-X-XXX-XX-XXX.ap-south-1.compute.amazonaws.com
  ```
- You may be prompted to confirm the connection; type **yes** and press Enter.

#### **Step 3: Complete the OpenVPN Setup**
- Once connected, OpenVPN Access Server installation will automatically begin. Simply press Enter when prompted to allow the installation process to complete.
- After the installation is finished, a terminal message will display the admin password for the OpenVPN Access Server. Copy this password.

---

### **3. Configure OpenVPN Access Server**

#### **Step 1: Access the Admin Interface**
- Open your web browser and go to the following URL to access the OpenVPN Access Server admin page:
  ```
  https://<PublicIP>:943/admin
  ```
    - Replace `<Public IP>` with the public IP of your EC2 instance.
    - For example: `https://X.XXX.XX.XXX:943/admin`.

#### **Step 2: Log in to Admin Panel**
- Use the following credentials to log in:
  - **Username:** openvpn
  - **Password:** The password you copied from the terminal after SSH login.

#### **Step 3: Configure VPN Settings**
- Once logged in, navigate to the **Configuration** tab.
- Under **VPN Settings**, enable the option: **Should client Internet traffic be routed through the VPN?**
- Click **Save** and then click **Update Running Server** to apply the changes.

---

### **4. Connect to the VPN Using OpenVPN Client**

#### **Step 1: Access the VPN Portal**
- Open your web browser and go to the following URL to access the OpenVPN client portal:
  ```
  https://<PublicIP>:943/
  ```
    - Replace `<Public IP>` with your EC2 instance’s public IP (without `/admin`).

#### **Step 2: Log in to the Client Portal**
- Use the following credentials to log in:
  - **Username:** openvpn
  - **Password:** The password you copied from the terminal after SSH login.

#### **Step 3: Download OpenVPN Client**
- After logging in, the client portal will prompt you to download the OpenVPN Desktop Client for your operating system.
- Download and install the client.

#### **Step 4: Connect to the VPN**
- Open the OpenVPN client that was installed.
- Use the client to connect to the VPN server.

#### **Step 5: Verify Your IP**
- After connecting to the VPN, search for your IP location online.
- The IP address shown should now reflect the VPN’s public IP.

---

### **5. Conclusion**

You have successfully deployed and connected to a VPN server hosted on AWS using OpenVPN. This setup will route your internet traffic through the VPN server, ensuring that your online activities remain secure and private.

Enjoy your secure and private connection!

---

### **6. Troubleshooting**

If you encounter issues, consider the following steps:
- Ensure that the security group rules are configured correctly to allow inbound traffic on the required ports.
- Double-check the EC2 instance's public IP address and the URL format used for the admin and client interfaces.
- Make sure your OpenVPN client is up-to-date.
- If the VPN connection fails, check the OpenVPN Access Server logs for error messages.

This documentation serves as a guideline for hosting an OpenVPN server on AWS and should be updated based on changes in AWS services, OpenVPN, or system configurations.

--- 

![image](https://github.com/user-attachments/assets/284a2e63-bfdd-46ed-8420-d77be6844d9d)
