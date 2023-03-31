---
Title: "Mini project reflection"
Date: 2023-03-31
---

<h1> Mini project reflection</h1>
  
  <p> In this project we have two different applications onc in Rust and other in Php. </p>
  
  <p> I was able to create an ec2 in cloud (aws) where these application are deployed using a shell spript developed by our team. 
   This ec2 instance is create using infrastructure as code using terraform.
    i create a basing ec2 using below terraform code.
  </p>
  
```
    resource "aws_instance" "phpInstance" {
      ami           = "ami-0c94855ba95c71c99" # Change to the AMI of your choice
      instance_type = "t2.micro"

      key_name = "spring22"
      tags = {
        Name = "example-instance"
      }
      vpc_security_group_ids = [aws_security_group.inbound.id]
    }

```
    
    
 <h3> Additional skills to learn </h3>
    
 <p> For the above ec2 instance, we need to create inbound and outbound rules so in order to expost tcp and ssh ports which i didnt try out before.</p>
   
   
```
    resource "aws_security_group" "inbound" {
      name_prefix = "inbound-sg-"


      ingress {
        from_port   = 22
        to_port     = 22
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
      }

      ingress {
        from_port   = 3000
        to_port     = 3000
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
      }
      ingress {
        from_port   = 80
        to_port     = 80
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
      }
    }
   
```
  
    
<h3> Challenges faced</h3>
    <p> Git issues were new to me, so using it was a bit challenging and coordnating with the team was a diffuclt task as it was a blocker till one finishes his task.<p>
