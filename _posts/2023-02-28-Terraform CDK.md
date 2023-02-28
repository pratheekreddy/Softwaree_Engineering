---
Title: "Terraform CDK"
Date: 2023-02-28
---

<h1> Terraform CDK </h1>

Terraform CDK (Cloud Development Kit) is an open-source software development framework that helps to deploy infrastructure resources on AWS and other cloud providers. IaC can be configured in languages like
Typescript, python, java, etc. instad of HashiCorp Configuration Language.

<h3> Prerequisites</h3>

`Node.js`

`Terraform cli`

<h3>Install Terraform CDK </h3>

```
npm install -g aws-cdk
```

<h3> Then i initialized the project with typescript as language</h3>

  ```
  cdk init app --language=typescript
```
Then i tried to create a simple s3 bucket in aws with this code
```
import * as cdk from 'aws-cdk-lib';
import * as s3 from 'aws-cdk-lib/aws-s3';

export class MyStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    new s3.Bucket(this, 'test', {
      versioned: true,
    });
  }
}
```

Run the following command to generate the Terraform configuration files

```
cdk synth
```

Run the following command to deploy the infrastructure
```
cdk deploy
```
