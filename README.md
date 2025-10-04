# Altschool Cloud Engineering Project - EC2 Website Deployment

This project automates the deployment of a website on EC2 instances using Ansible, NGINX, and Application Load Balancer.

## Project Structure

ansible-web/
├── inventory/
│   └── hosts                 # Ansible inventory file
├── templates/
│   ├── index.html.j2        # HTML template showing server IP
│   └── nginx-default.j2     # NGINX configuration template
├── playbook.yml             # Main deployment playbook
├── alb-prep.yml            # ALB preparation playbook
├── ansible.cfg             # Ansible configuration
└── README.md               # This file


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

### Step 4: Prepare for Application Load Balancer

```bash
ansible-playbook alb-prep.yml
```

## Features

- ✅ Automated NGINX installation and configuration
- ✅ Website deployment showing server IP address
- ✅ Health check endpoint for load balancer (`/health`)
- ✅ Proper file permissions and ownership
- ✅ Service management (start/enable NGINX)
- ✅ Firewall configuration

## Verification

After running the playbook, verify deployment:

1. Check website: `http://YOUR_EC2_IP`
2. Check health endpoint: `http://YOUR_EC2_IP/health`

## Next Steps for ALB Setup

1. Create Application Load Balancer in AWS Console
2. Create Target Group with your EC2 instances
3. Configure health checks to use `/health` endpoint
4. Update security groups to allow ALB traffic

## Troubleshooting

- Check Ansible logs: `/tmp/ansible.log`
- Verify NGINX status: `sudo systemctl status nginx`
- Check NGINX error logs: `sudo tail -f /var/log/nginx/error.log`
