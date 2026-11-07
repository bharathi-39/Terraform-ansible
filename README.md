# Terraform + Ansible AWS Automation Project

<img width="1077" height="671" alt="image" src="https://github.com/user-attachments/assets/6bb4dbbf-a6c6-4216-94a9-ecc8ec774093" />


A resume and GitHub portfolio project for a DevOps Engineer, built with Terraform, Ansible, AWS, and GitHub.

## Project Goal

**Terraform will:**
- Create VPC
- Create Public Subnet
- Create Internet Gateway
- Create Route Table
- Create Security Group
- Create EC2 Instance

**Ansible will:**
- Connect to EC2
- Install Nginx
- Deploy a sample website

**GitHub will store:**
- All Terraform code
- All Ansible playbooks
- Documentation

---

## Step 1: Prerequisites

Install:
- AWS CLI
- Terraform
- Ansible
- Git
- VS Code

Configure AWS:

```bash
aws configure
```

Enter:
- Access Key
- Secret Key
- Region (`ap-south-1`)
- Output (`json`)

Verify:

```bash
aws sts get-caller-identity
```

---

## Step 2: Create GitHub Repository

Create a repository named:

```
terraform-ansible-aws-automation
```

Clone it:

```bash
git clone https://github.com/<username>/terraform-ansible-aws-automation.git
cd terraform-ansible-aws-automation
```

---

## Step 3: Create Folder Structure

```
terraform-ansible-aws-automation
│
├── terraform
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── provider.tf
│
├── ansible
│   ├── inventory.ini
│   ├── install_nginx.yml
│   └── deploy_site.yml
│
├── website
│   └── index.html
│
└── README.md
```

---

## Step 4: Terraform Provider

`provider.tf`

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

---

## Step 5: Create AWS Network

`main.tf`

Create:
- VPC
- Subnet
- Internet Gateway
- Route Table
- Security Group

Security Group rules:
- 22 (SSH)
- 80 (HTTP)

Open from: `0.0.0.0/0`

---

## Step 6: Launch EC2

Create:

```hcl
resource "aws_instance" "web"
```

Use:
- Amazon Linux 2
- `t2.micro`

Attach:
- Public subnet
- Security group

Generate output:

```hcl
output "public_ip"
```

---

## Step 7: Deploy Infrastructure

```bash
terraform init
terraform validate
terraform plan
terraform apply
```

Check the output for the **Public IP** and save it.

---

## Step 8: Test EC2 Access

```bash
ssh -i key.pem ec2-user@PUBLIC-IP
```

Verify:

```bash
hostname
```

---

## Step 9: Configure Ansible Inventory

`inventory.ini`

```ini
[webserver]
13.x.x.x ansible_user=ec2-user ansible_ssh_private_key_file=key.pem
```

Test:

```bash
ansible all -i inventory.ini -m ping
```

Expected: `SUCCESS`

---

## Step 10: Create Nginx Playbook

`install_nginx.yml`

Tasks:
- Install nginx
- Start nginx
- Enable nginx

Run:

```bash
ansible-playbook -i inventory.ini install_nginx.yml
```

Verify by opening `http://PUBLIC-IP`. The Nginx page should appear.

---

## Step 11: Create Website

`index.html`

```html
<h1>Hi All!</h1>
<h2>Terraform + Ansible Project</h2>
```

---

## Step 12: Deploy Website Using Ansible

`deploy_site.yml`

Tasks:
- Copy `index.html`
- Restart nginx

Run:

```bash
ansible-playbook -i inventory.ini deploy_site.yml
```

Access `http://PUBLIC-IP`. You should see:

```
Hi All!
Terraform + Ansible Project
```

---

## Step 13: Push Everything to GitHub

```bash
git add .
git commit -m "Terraform AWS Infrastructure Automation Project"
git push origin main
```

---

## Step 14: Create an Attractive README

Include:

**Project Architecture**

**Tools Used:**
- AWS
- Terraform
- Ansible
- GitHub

**Resources Created:**
- VPC
- Public Subnet
- Internet Gateway
- Route Table
- Security Group
- EC2

**Automation:**
- Infrastructure provisioning using Terraform
- Configuration management using Ansible
- Website deployment using Ansible

**Screenshots to add:**
- Terraform Apply
- AWS Console
- EC2 Running
- Nginx Website
- GitHub Repository

---

## Step 15: Resume Project Description

**AWS Infrastructure Automation using Terraform & Ansible**

- Automated AWS infrastructure provisioning using Terraform following Infrastructure as Code (IaC) principles.
- Created VPC, Public Subnet, Internet Gateway, Route Tables, Security Groups, and EC2 instances.
- Developed reusable Terraform configurations for consistent deployments.
- Automated server configuration and Nginx deployment using Ansible playbooks.
- Deployed a sample web application on AWS EC2 through Ansible automation.
- Managed source code and playbooks using Git and GitHub.
- Implemented AWS security best practices including Security Groups and IAM-based access control.

---

## Next Project Idea

After this, a **Terraform + Jenkins + Ansible + AWS CI/CD pipeline** project is a strong interview-level follow-up.
