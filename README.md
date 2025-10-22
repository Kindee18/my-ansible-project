# Altschool Cloud Engineering Project - EC2 Website Deployment

This project automates the deployment of a website on EC2 instances using Ansible, NGINX, and Application Load Balancer.

## Reference

For a detailed tutorial on this project, check out the Medium article: [Deploy a Website on EC2 Instances with Automation](https://medium.com/@kindsonegbule15/deploy-a-website-on-ec2-instances-with-automation-7235dadbe5e4)

## Project Structure

my-ansible-project/
├── inventory/
│ └── hosts # Ansible inventory file
├── playbook.yml # Main deployment playbook
├── ansible.cfg # Ansible configuration
├── index.html # Universal HTML file (works on EC2, ALB, and S3)
└── README.md # This file

## Prerequisites

1. ✅ Two EC2 instances launched (Ubuntu/Amazon Linux)
2. SSH key pair for accessing instances
3. Ansible installed on local machine
4. Security groups allowing HTTP (port 80) and SSH (port 22)

## Setup Instructions

### Step 1: Update Inventory File

Edit `inventory/hosts` with your actual EC2 instance details:

```ini
[webservers]
web1 ansible_host=YOUR_EC2_INSTANCE_1_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem
web2 ansible_host=YOUR_EC2_INSTANCE_2_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem
```

### Step 2: Test Connectivity

```bash
ansible all -m ping
```

### Step 3: Deploy Website with NGINX

```bash
ansible-playbook playbook.yml
```

The playbook will:

- Install and configure NGINX on both servers
- Deploy the universal HTML file that works everywhere
- Each server will show its own IP address dynamically
- Create health check endpoint for load balancer
- Use default NGINX configuration (simple and reliable)

### Step 4: Deploy on S3

- Use AWS console to create an S3 bucket
- Move the web files to the bucket
- Configure permission issues
- Enable static hosting 

### Step 4: Set up Application Load Balancer

Set up ALB through AWS Console:

1. Create Target Group with your EC2 instances
2. Create Application Load Balancer
3. Configure health checks to use `/health` endpoint
4. Update security groups to allow ALB traffic

## Features

- ✅ Automated NGINX installation and configuration
- ✅ Universal HTML file that works on EC2, ALB, and S3
- ✅ Health check endpoint for load balancer (`/health`)
- ✅ Proper file permissions and ownership
- ✅ Service management (start/enable NGINX)
- ✅ Firewall configuration

## Verification

After running the playbook, verify deployment:

1. Check website: `http://YOUR_EC2_IP`
2. Check health endpoint: `http://YOUR_EC2_IP/health`

## Project Completion Status

✅ **All project objectives completed successfully:**

1. ✅ Launch 2 EC2 instances on AWS (Free Tier)
2. ✅ Create and deploy HTML page showing server IP address
3. ✅ Use Ansible to install NGINX and automate deployment
4. ✅ Set up Application Load Balancer for traffic distribution

**ALB URL:** <http://my-first-alb-1239242874.eu-north-1.elb.amazonaws.com>

## Troubleshooting

- Check Ansible logs: `/tmp/ansible.log`
- Verify NGINX status: `sudo systemctl status nginx`
- Check NGINX error logs: `sudo tail -f /var/log/nginx/error.log`

---

## Universal HTML File Usage

- The **same HTML file** (`index.html`) is used for EC2, Load Balancer, and S3 deployments.
- No server-side code (like PHP or Jinja2) is required; all logic is handled via Bash scripting in the Ansible playbook using the `sed` command.
- For EC2 and Load Balancer, the page displays the actual serving instance's public IP address, which is injected during deployment using Bash scripting.
- For S3, the page will display the placeholder text as-is since no dynamic IP replacement occurs in static hosting.
- This ensures a consistent user experience and meets the project requirement to use the same file everywhere.

## How It Works

- **Direct EC2 Access:** Shows the public IP address of the EC2 instance, replaced by Bash scripting during deployment.
- **Load Balancer Access:** Shows the public IP address of the EC2 instance serving the request, replaced by Bash scripting during deployment.
- **S3 Static Website:** Shows the original placeholder text since no dynamic replacement occurs in S3 static hosting.

## Deployment Notes

- Always use the same `index.html` file for all environments.
- The IP address is dynamically replaced using Bash scripting in the Ansible playbook during deployment to EC2 instances.
- For S3, you will need to manually edit the HTML file to replace the placeholder with appropriate text before uploading.
- No need to maintain separate files for different environments.

---
