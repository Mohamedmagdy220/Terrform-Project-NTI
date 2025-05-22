# Terraform project

> **Fully Automated AWS Infrastructure with Terraform**  
> Deploys a modular, highly available VPC network, bastion (proxy) & backend EC2 instances, and both Internet-facing and internal load balancers.

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)  
2. [Architecture Diagram](#architecture-diagram)  
3. [Getting Started](#getting-started)  
   - [Prerequisites](#prerequisites)  
   - [Directory Layout](#directory-layout)  
4. [Configuration](#configuration)  
   - [Variables](#variables)  
   - [Terraform Backend](#terraform-backend)  
5. [Usage](#usage)  
   1. [Initialize](#initialize)  
   2. [Validate](#validate)  
   3. [Plan](#plan)  
   4. [Apply](#apply)  
   5. [Destroy](#destroy)  
6. [Modules Breakdown](#modules-breakdown)  
7. [Inputs & Outputs](#inputs--outputs)   
8. [Contributing](#contributing)  
---

## Project Overview

This repository defines an AWS environment that includes:

- **VPC** with multiple public and private subnets across availability zones  
- **Bastion Host** (Proxy) in public subnets for secure SSH access  
- **Backend EC2 Instances** in private subnets running your application  
- **Internet-facing Classic ELB** routing to public instances  
- **Internal Application Load Balancer** routing to private backends  

Everything is built using reusable Terraform modules to ensure consistency, maintainability, and easy scaling.

---

## Architecture Diagram

.
├── modules             /
│ ├── vpc               / ← Creates VPC + Internet Gateway + NAT Gateways
│ ├── subnet           / ← Public & private subnets per AZ
│ ├── security_group   / ← SGs for bastion, backends, ELBs
│ ├── ec2              / ← Launch bastion & backend servers
│ ├── elb              / ← Internet-facing Classic ELB
│ └── internal-alb     / ← Internal Application Load Balancer
├── main.tf              ← Root module wiring everything together
├── variables.tf         ← All input variable definitions
├── outputs.tf           ← Exported outputs (IDs, DNS names)
├── backend.tf           ← Remote state configuration
└── terraform.tfvars     ← Your environment-specific values

---

## Getting Started ##

### Prerequisites

- Terraform v\<0.13\> or later  
- AWS CLI v2 configured with an IAM user/role that can create VPCs, EC2, ELB, S3 & DynamoDB  
- An existing S3 bucket & DynamoDB table for remote state locking


## Configuration

### Variables

Define defaults in `variables.tf` and override in `terraform.tfvars`:

## Terraform Backend

Configure remote state in backend.tf

## Usage

Run these commands from the repo root:

1- terraform init
2- terraform validate
3- terraform plan
4- terraform apply


##  Modules Breakdown

Module	                           Purpose
vpc	                  Creates VPC, IGW, NAT Gateways, route tables
subnet	              Provisions public & private subnets across AZs
security_group	      Defines security rules for bastion, backends, ELBs
ec2	                  Launches bastion and backend instances with user data
elb	                  Sets up an external Classic ELB for public access
internal-alb	        Chooses an internal ALB for routing to private backends


## Inputs & Outputs
## Inputs

Name	                            Description	                           Type	              Required
project_name	            Prefix/tag for all resources	                string	               yes
environment	              Deployment environment (dev, prod)	          string	               yes
vpc_cidr	                CIDR block for VPC	                          string	               yes
public_subnet_cidrs	      List of public subnet CIDRs per AZ	           list	                 yes
private_subnet_cidrs	    List of private subnet CIDRs per AZ	           list	                 yes

## Outputs

Name                          	Description
vpc_id	                 The ID of the created VPC
public_subnet_ids	       List of public subnet IDs
private_subnet_ids	     List of private subnet IDs
bastion_public_ips	     Public IP addresses of bastion hosts
elb_dns_name	           DNS name of Internet-facing ELB
internal_alb_dns_name    DNS name of internal Application Load Balancer


############ Contributing ###############
1-> Fork this repository

2-> Create a feature branch: git checkout -b feature/<feature-name>

3-> Commit your changes: git commit -m "Add <feature>"

4-> Push: git push origin feature/<feature-name>

5-> Open a Pull Request and tag a reviewer
