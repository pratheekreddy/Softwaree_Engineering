---
Title: "Introduction to Terraform"
Date: 2023-02-01
---

#### What is Terraform?
A. It is specifically used as an infrastructure provisioning tool. It allows you to define and manage your infrastructure as code, making it easy to  automate, and manage with less human errors while creating everythin manually. Though terraform is a tool, the language used is HCL i.e. HashiCorp Configuration language.

It supports multiple clou providers including private, public and on-premise infrastructure.

#### Installing Terraform

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

sudo apt-get install terraform
```

#### Terraform on AWS

To create IaC for AWS we need to install aws cli and configure aws credientials which can be obtained for IAM.

Then start writing code in .tf extension files.

#### First Code

Task is to create a simple ec2 instance with t2 micro 

```
//test.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "test_instance" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "test_tag"
  }
}
```

Initilize workspace

```
terraform init
```
To Preview changes and all actions that terraforms will perform, use below command
```
terraform plan
```

To Deploy your code i.e. to create the infrastructure on AWS, use below command
```
terraform apply
```

We can track the status by using
```
terraform show
```
