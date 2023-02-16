---
Title: "Terraform CLI"
Date: 2023-02-012
---

<h1>Terraform CLI </h1>

`terraform` command in cli gives list of available commands.

To get information about a specfic command we can use `-help` ex: `terraform validate -help` this tells what validate command does.

To initialize a terraform direatory use `terraform init`. this creates the hidden .terraform directory that contains information about providers, modules, etc.

`terraform plan` evalutes the plan based on the terraform iac. This comapes the json file created by the code and the remote provider state. Suppose if you as to create A, B, C resourses in
terraform, and the provider already has A and C resourse, it will plat to create only B resourse insted of all three.

To execute the deployment based on the configuration we need to use `terraform plan` command. 

`terraform apply command takes the plan and executed it in the remote location. We can run apply and plan before initializing terraform.

The `terraform get` command will download modules referenced in the configuration, but will not perform the other required initialization tasks.

The `terraform destroy` command destroys all of the resources being managed by the current working directory.

The `terraform graph` command is used to generate a visual representation of either a configuration or execution plan.

The `terraform output` command is used to extract the value of an output variable from the state file.

The `terraform show` command is used to provide human-readable output from a state or plan file.


<h3>Challanges faced </h3>

Initially faced issue while installing terraform cli in ubuntu as my laptop was running in insecure mode(turned it off as i was facing challanges with wifi). when i turned it to secured mode, it installed perfectly fine.
