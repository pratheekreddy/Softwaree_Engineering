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

Terraform CDK can be used to set up automated backup and recovery processes using S3. we can use Terraform CDK to create and configure S3 buckets for backup storage, and set up automated backup and recovery scripts using Lambda functions or other AWS services.

```
import * as aws from 'aws-cdk-lib/aws';
import * as cdk from 'aws-cdk-lib';

export class BackupStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // Create an S3 bucket for backup storage
    const backupBucket = new aws.s3.Bucket(this, 'BackupBucket', {
      versioned: true,
      lifecycleRules: [
        {
          expiration: cdk.Duration.days(30),
          transitions: [
            {
              storageClass: aws.s3.StorageClass.INFREQUENT_ACCESS,
              transitionAfter: cdk.Duration.days(7)
            },
            {
              storageClass: aws.s3.StorageClass.GLACIER,
              transitionAfter: cdk.Duration.days(14)
            }
          ]
        }
      ]
    });

    // Create an S3 bucket policy to restrict access to the backup bucket
    const backupBucketPolicy = new aws.s3.BucketPolicy(this, 'BackupBucketPolicy', {
      bucket: backupBucket.bucketName,
      policy: {
        Version: '2012-10-17',
        Statement: [
          {
            Effect: 'Deny',
            Principal: '*',
            Action: ['s3:*'],
            Resource: [
              backupBucket.arnForObjects('*')
            ],
            Condition: {
              StringNotEquals: {
                's3:x-amz-server-side-encryption': 'AES256'
              }
            }
          }
        ]
      }
    });
  }
}

```

<h1> Challanges faced</h1>

initially faced issues while installing cdk in ubuntu as something is wrong with my package manager so i had to shift to mac.
