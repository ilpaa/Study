![image](https://github.com/user-attachments/assets/84e4c407-ac23-4573-a056-de07cf0b25c3)# AWS VPC Web Server Hosting

## Overview
This project demonstrates how to create a custom AWS Virtual Private Cloud (VPC) to host a website. The VPC includes public and private subnets across multiple availability zones. An EC2 instance is launched in the public subnet, and a security group is configured to allow HTTP traffic for website hosting.
![image](https://github.com/user-attachments/assets/97d89698-cf7c-409b-b77e-82ba321b150f)


The website hosted in this lab is a project I created. The entire setup follows AWS best practices, ensuring security and high availability.

## Objectives
- Create a custom VPC with subnets in two Availability Zones
- Configure a NAT Gateway for internet access from the private subnet
- Set up security groups to allow HTTP access
- Launch an EC2 instance and deploy a web server
- Host a custom website on the EC2 instance

## Architecture
The architecture consists of:
- A VPC with both public and private subnets
- Route tables for the subnets
- An Internet Gateway and NAT Gateway
- An EC2 instance running in the public subnet with a web server
- A custom web page hosted on the EC2 instance

## Steps
### Creating a VPC

![image](https://github.com/user-attachments/assets/463274f5-9efe-4123-b103-7975806bac77)

1. **Begin creating a VPC**:
   - In the top-right of the screen, verify that **N. Virginia (us-east-1)** is the region.
   - Choose the **VPC dashboard** link, located toward the top left of the console.
   - Next, choose ![image](https://github.com/user-attachments/assets/6445f67e-8790-42aa-9066-d906c0d9258e)
.
     - _Note: If you do not see a button with that name, choose the **Launch VPC Wizard** button instead._

2. **Configure the VPC details in the VPC settings panel**:
   - Choose **VPC and more**.
   - Under **Name tag auto-generation**, keep **Auto-generate** selected, but change the value from `project` to `lab`.
   - Keep the **IPv4 CIDR block** set to `10.0.0.0/16`.
   - For **Number of Availability Zones**, choose `1`.
   - For **Number of public subnets**, keep the setting as `1`.
   - For **Number of private subnets**, keep the setting as `0`.

3. **Expand the Customize subnets CIDR blocks section**:
   - Change **Public subnet CIDR block** in `us-east-1a` to `10.0.0.0/24`.

4. **Set other options**:
   - Set **NAT gateways** to `None`.
   - Set **VPC endpoints** to `None`.
   - Keep both **DNS hostnames** and **DNS resolution** enabled.

Once the creation is complete, choose ![image](https://github.com/user-attachments/assets/fad0bca4-6870-4334-8e86-582e04b71f37)


The wizard will have provisioned a VPC with a public subnet and a private subnet in one Availability Zone, with route tables for each subnet. It will also have created an Internet Gateway. To view the settings of these resources, browse through the VPC console links, such as the **Subnets** link to view subnet details or the **Route tables** link to view the route table details.

![image](https://github.com/user-attachments/assets/0c62f86e-5126-48da-a1a7-a9218e1095c9)

# Launching an EC2 Instance and Hosting a Web Application
![image](https://github.com/user-attachments/assets/6007bf2e-39e2-4eed-b1fd-73bfe5853c9d)

## Navigate to EC2 Dashboard:
- From the AWS Management Console, select **EC2** under the Compute section.

### Launch Instance:
1. Click **Launch Instance**.
2. Name your instance (e.g., `MyWebAppServer`).
3. Choose an Amazon Machine Image (AMI) (e.g., `Amazon Linux 2`).
4. Select an Instance Type (e.g., `t2.micro` for free tier).
5. Configure the Key Pair for SSH access (create a new one if needed).

![image](https://github.com/user-attachments/assets/4fd03f9f-cebc-409f-b88b-608076c26499)

## Network Settings:
1. Under **Network settings**, choose the default VPC and Subnet.
2. Ensure that **Auto-assign Public IP** is enabled.
3. In **Security Groups**, create a new one to allow traffic:
   - Allow **HTTP** (port 80) for web traffic.
   - Allow **SSH** (port 22) for secure access.
   - Allow **HTTP** (port 80).
   - Allow **HTTPS** (port 443) if you are serving a secure site.

## Configure Storage:
- Set the Root Volume (default: 8 GB, General Purpose SSD).

## Script to Create a Simple Web Server
Instead of using SSH manually, you can add a script during the EC2 instance creation process in the **Advanced Details** section. This script will automatically install and start a simple web server.

##Expand  Advanced details.

Scroll to the bottom of the page and then copy and paste the code shown below into the User data box:
![image](https://github.com/user-attachments/assets/e8b6acb7-4458-4971-9b1f-bcb059b32386)

### Here's a script that you can use to host a simple web application:

```bash
#!/bin/bash
# Update the system
yum update -y

# Install Apache
yum install -y httpd

# Start Apache
systemctl start httpd
systemctl enable httpd

# Create a simple web page
echo "<html><body><h1>Welcome to My Web Application</h1></body></html>" > /var/www/html/index.html

```
Next Choose, ![image](https://github.com/user-attachments/assets/b4a8621f-5c83-45b8-b59b-c355dffc9113)


## Test the Web Server
After your EC2 instance has launched and the script has run, you can test the web server to ensure everything is working correctly.

### Steps to Test:

1. **Obtain the Public IP Address:**
   - Navigate to the **EC2 Dashboard**.
   - Select your instance and find the **Public IPv4 address** in the instance details.
![Screenshot 2024-09-08 185841](https://github.com/user-attachments/assets/00a4972a-f1b8-427c-83b6-1ffc199014a4)


2. **Access the Web Application:**
   - Open a web browser.
   - Enter the public IP address of your EC2 instance in the address bar (e.g., `http://your-public-ip`).

3. **Verify the Web Page:**
   - If the setup was successful, you should see a webpage displaying: `Welcome to My Web Application`.

### Troubleshooting:
- If the webpage doesn't load:
  - Verify that your security group allows HTTP traffic on port 80.
  - Ensure the instance is running and accessible.
  - Recheck the script in the **User Data** section for any errors.

This test confirms that your web server is running correctly and serving the web application as expected.



