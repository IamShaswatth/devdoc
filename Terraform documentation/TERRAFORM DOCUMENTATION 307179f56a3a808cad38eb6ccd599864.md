# TERRAFORM DOCUMENTATION

### ***objective***

The objective of this learning is to provision cloud infrastructure using Terraform as an Infrastructure as Code (IaC) tool, enabling automated, repeatable, and scalable deployments.

### ***About terraform***

Terraform is an open-source Infrastructure as Code tool developed by HashiCorp used to provision and manage cloud resources using declarative configuration files.

### ***Installation***

```bash
sudo apt update
sudo apt install terraform -y

```

### ***Configuration***

The configuration is given and saved in main.tf

```bash
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default_in_vpc" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}

# Amazon Linux 2023 AMI (official AWS SSM parameter)
data "aws_ssm_parameter" "al2023_ami" {
  name = "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
}

# Create key pair only when you do NOT provide an existing key pair name
resource "aws_key_pair" "generated" {
  count      = var.existing_key_pair_name == "" ? 1 : 0
  key_name   = var.generated_key_pair_name
  public_key = file(pathexpand(var.public_key_path))
}

resource "aws_security_group" "ec2_sg" {
  name        = "terraform-ec2-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [var.allowed_ssh_cidr]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "terraform-ec2-sg"
  }
}

resource "aws_instance" "ec2" {
  ami                         = data.aws_ssm_parameter.al2023_ami.value
  instance_type               = var.instance_type
  subnet_id                   = data.aws_subnets.default_in_vpc.ids[0]
  vpc_security_group_ids      = [aws_security_group.ec2_sg.id]
  associate_public_ip_address = true

  # IMPORTANT: This must be AWS key pair name, not .pem file name.
  key_name = var.existing_key_pair_name != "" ? var.existing_key_pair_name : aws_key_pair.generated[0].key_name

  tags = {
    Name = "terraform-ec2"
  }
}
                      
```

Before applying terraform apply aws configure 

### *Installation aws configure*

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

 then press aws configure  and paste necessary keys 

```bash
aws configure
```

Terraform commands

```bash
terraform init

```

```bash
terraform plan

```

```bash
terraform apply

```

The above commands creates an EC2 instance in aws 

![Screenshot 2026-02-13 162058.png](Screenshot_2026-02-13_162058.png)

then apply 

```bash
terraform destroy
```

to terminate the instance