# AWS EC2 Practical Handbook: Detailed Step-by-Step Guide   <img src="https://github.com/user-attachments/assets/f16f083a-b300-4fa0-9a4b-21d4a471d1c6" alt="IAM Logo" width="40" />



## 1. Introduction to EC2

Objective: Understand what EC2 is and how it fits into the AWS ecosystem.

- EC2 Overview: EC2 (Elastic Compute Cloud) is a scalable computing service provided by AWS that allows you to run virtual machines (known as instances) in the cloud. These instances can be configured with different resources (CPU, RAM, storage) to suit the needs of your applications.
  
- Key Components:
  - AMI (Amazon Machine Image): Template for launching an instance.
  - Instance Types: Different configurations (e.g., `t2.micro`, `t2.medium`) for varying CPU, memory, and storage needs.
  - Security Groups: A virtual firewall that controls traffic to instances.
  - Elastic IP: A static IP address for an EC2 instance that can be reassigned.
  - Elastic Block Store (EBS): Persistent storage that can be attached to EC2 instances.



## 2. Launching an EC2 Instance

Objective: Launch your first EC2 instance.

### Step 1: Sign in to AWS Console
- Open your browser and navigate to the [AWS Management Console](https://aws.amazon.com/console/).
- Enter your credentials to log in. If you don’t have an AWS account, you’ll need to create one.
  
### Step 2: Navigate to EC2 Dashboard
- Once logged in, in the Find Services search bar, type “EC2” and select EC2 under the "Compute" section.

### Step 3: Launch an EC2 Instance
- On the EC2 Dashboard, click the Launch Instance button.
  
### Step 4: Select an AMI
- Choose Amazon Linux 2 AMI (HVM), SSD Volume Type from the list of available AMIs. Amazon Linux is a good choice for most basic applications.
  
### Step 5: Choose an Instance Type
- For free-tier users, select t2.micro. This instance type is eligible for the free tier and provides 1 vCPU and 1GB of RAM, which is enough for small tasks and practice.
- After selecting the instance type, click Next: Configure Instance Details.

### Step 6: Configure Instance Details
- For a simple setup, you can leave the default settings as they are.
  - Network: Select the default VPC (Virtual Private Cloud).
  - Subnet: Choose Auto-assign Public IP so your instance can be accessed via the internet.
  - Leave IAM role and other settings as default for now.

### Step 7: Add Storage
- By default, Amazon Linux 2 instances are allocated 8GB of EBS storage. You can leave this as it is for now, or you can increase the size by clicking on Add New Volume and entering a larger size, such as 20GB.
  
### Step 8: Add Tags
- Tags are optional. You can add a tag to make identifying your EC2 instance easier later.
  - For example, create a tag with Key as `Name` and Value as `MyFirstInstance`.

### Step 9: Configure Security Group
- Create a new security group or choose an existing one. If creating a new one:
  - Add a Rule to allow SSH on port 22 so you can connect to the instance.
  - Add a Rule to allow HTTP on port 80 if you plan to install a web server.
  - SSH should be open only to your IP address to keep it secure.

### Step 10: Review and Launch
- Review the instance configuration. Once satisfied, click Launch.
- You’ll be prompted to create a new Key Pair for SSH access. Download and save the .pem file in a safe location (you won’t be able to download it again).
  - Click Launch Instances to finalize.
  
### Step 11: Accessing Your Instance
- Go back to the EC2 Dashboard, and you should see your newly launched instance.
- Copy the Public IP Address or Public DNS.
- Open your terminal, navigate to the folder where you saved the `.pem` key file, and run:
  ```bash
  ssh -i /path/to/my-keypair.pem ec2-user@<EC2_PUBLIC_DNS>
  ```
  This command will connect you to your EC2 instance via SSH.


## 3. Associating Elastic IP with EC2

Objective: Associate a static IP (Elastic IP) to your EC2 instance.

### Step 1: Allocate Elastic IP
- In the EC2 Dashboard, on the left-hand menu, click Elastic IPs under Network & Security.
- Click Allocate Elastic IP address and then click Allocate. This will create a new Elastic IP.

### Step 2: Associate Elastic IP with EC2 Instance
- Select the newly created Elastic IP, click Actions > Associate Elastic IP address.
- In the Instance section, select your EC2 instance from the drop-down list and click Associate.

### Step 3: Test the Elastic IP
- Open a browser and enter the new Elastic IP. The Apache web server should be accessible at this new static IP address.



## 4. Adding EBS Volume to EC2

Objective: Attach an additional EBS volume and mount it.

### Step 1: Create EBS Volume
- In the EC2 Dashboard, click Volumes under Elastic Block Store in the left-hand menu.
- Click Create Volume, and select a size (e.g., 10GB) and the same availability zone as your EC2 instance.

### Step 2: Attach the EBS Volume
- Once the volume is created, select it, click Actions > Attach Volume.
- Choose your EC2 instance and click Attach.

### Step 3: Format and Mount the Volume
- SSH into your EC2 instance, and run the following commands:
  ```bash
  sudo mkfs -t ext4 /dev/xvdf  # Replace xvdf with your volume identifier
  sudo mkdir /data             # Create a directory to mount the volume
  sudo mount /dev/xvdf /data   # Mount the volume
  ```

### Step 4: Verify Volume Mounting
- Run:
  ```bash
  df -h
  ```
  You should see the new volume mounted under `/data`.



## 5. Configuring Auto Scaling

Objective: Set up Auto Scaling to automatically adjust the number of EC2 instances based on demand.

### Step 1: Create Launch Configuration
- In the EC2 Dashboard, under Auto Scaling > Launch Configurations, click Create launch configuration.
- Choose an AMI (such as the one used for your EC2 instance), select an instance type (e.g., `t2.micro`), and configure it to match your desired instance settings.
  
### Step 2: Create Auto Scaling Group
- After creating the launch configuration, go to Auto Scaling Groups and click Create Auto Scaling Group.
- Configure the group to launch instances based on scaling policies (e.g., minimum of 1 instance, maximum of 3 instances).
  
### Step 3: Create Scaling Policy
- Set policies to automatically scale the number of instances based on CPU utilization, like adding an instance if CPU utilization exceeds 70% for 5 minutes.
  
### Step 4: Attach Load Balancer
- Attach an Elastic Load Balancer (ELB) to your Auto Scaling group to distribute traffic among instances.



## 6. Monitoring EC2 with CloudWatch

Objective: Monitor your EC2 instance’s performance.

### Step 1: Enable Detailed Monitoring
- In the EC2 Dashboard, under Instances, select your EC2 instance, click Actions > Monitor and troubleshoot > Enable detailed monitoring.

### Step 2: Create a CloudWatch Alarm
- Go to the CloudWatch dashboard, click Alarms > Create Alarm.
- Choose EC2 > Per-Instance Metrics and select CPUUtilization.
- Set the condition (e.g., trigger alarm if CPU exceeds 80% for 5 minutes).
- Add an action like sending an email or triggering an SNS notification.

