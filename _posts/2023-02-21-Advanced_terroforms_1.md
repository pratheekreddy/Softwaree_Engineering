---
Title: "Advance Terraform"
Date: 2023-02-21
---

<h1> Advance Terraform </h1>

This week i tried something different and challanging. First I studied about aws SQS(Simple queue service) which is a fully managed message queuing service that enables decoupling of microservices.

Generally, a lambda function can be triggered with the help of sqs. So, when ever there is a new message on the queue, lambda function can consume it.

So, i tried implementing this in terraforms.

First, i created a sqs in terraform
```
provider "aws" {
  region = "us-west-2"
}

resource "aws_sqs_queue" "my_queue" {
  name                      = "test-queue"
  visibility_timeout_seconds = 600
}
```

Then created a lambda function.

```
resource "aws_lambda_function" "my_lambda" {
  filename         = "/Desktop/lambda/lambda_test.zip"
  function_name    = "test-lambda"
  role             = "${aws_iam_role.lambda_role.arn}"
  handler          = "lambda_test.lambda_handler"
  runtime          = "python3.8"
  source_code_hash = "${filebase64sha256("lambda_test.zip")}"
  environment {
    variables = {
      SQS_QUEUE_URL = "${aws_sqs_queue.my_queue.url}"
    }
  }
}
```
we need to make sure our lambda function code to be zipped and placed in correct location.

Then i defined a role for lambda
```
resource "aws_iam_role" "lambda_role" {
  name = "lambda-role"
  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      }
    }
  ]
}
EOF
}
```

Then i assigned the lambda basic execution role to sqs
```
resource "aws_iam_role_policy_attachment" "lambda_role_policy" {
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
  role       = "${aws_iam_role.lambda_role.name}"
}

resource "aws_lambda_event_source_mapping" "my_lambda_mapping" {
  event_source_arn = "${aws_sqs_queue.my_queue.arn}"
  function_name    = "${aws_lambda_function.my_lambda.arn}"
}
```

<h2> Challanges faced</h2>

Initially i was facing errors while creating a trigger for lambda because of the permissions. later this got fixed whin i added role in terraforms.
