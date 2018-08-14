AWS Web App
======

Resources for quickly deploying the AWS Serverless Web Application.  Mostly this was used to teach myself AWS tools, including CloudFormation, Lambda, API Gateway, and more.

[Build a Serverless Web Application](https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/)

## Software Requirements

You'll need the following software installed to get started.

  * [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html): Run AWS commands from your local terminal

## Accounts

AWS account

[AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html): If you have multiple AWS profiles, you must specify which profile to use by appending the parameter "--profile <profile_name>" to the AWS CLI commands below.

## Getting Started

Clone this repository.

```
git clone https://github.com/nate-b/aws-web-app
```

Run package command.  Note the s3 bucket is just a temporary location to store the yaml to execute.  It may not even be persisted (need to confirm).
```
aws cloudformation package --template-file template.yaml --output-template-file aws-web-app-output.yaml --s3-bucket cloudformation-templates-nate-baker
```

Deploy to AWS.

```
aws cloudformation deploy --template-file aws-web-app-output.yaml --stack-name myrydes-nate-baker --capabilities CAPABILITY_NAMED_IAM
```

Upload the static website files to the S3 bucket.

```
aws s3 sync ./website s3://myrydes-nate-baker
```

Validate your website using the url found in the stack output ([see "How to check your CloudFormation stack output" below](#stack-output)).

Update website/js/config.js with your environment-specific values and push the changes to your S3 bucket.
  * userPoolId - from stack output
  * userPoolClientId - from stack output
  * region - your default region

```
aws s3 sync ./website s3://myrydes-nate-baker
```

Follow the last step at [this link](https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-2/) to test your implementation.

### <a id="stack-output"></a>How to check your CloudFormation stack output.
You can view the Outputs in the AWS Console or use the command below.

```
aws cloudformation describe-stacks --stack-name myrydes-nate-baker
```

### Notes

If you want to remove config.js from version control.

```
git update-index --assume-unchanged website/js/config.js
```

### Next steps