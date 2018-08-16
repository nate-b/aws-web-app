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

Clone this repository:
```
git clone https://github.com/nate-b/aws-web-app
```

Run package command.  The s3 bucket specified is just a location to store the package artifacts used by your stack:
```
aws cloudformation package --template-file template.yaml --output-template-file aws-web-app-output.yaml --s3-bucket temp-stack-artifacts-nate-baker
```

Deploy to AWS:
```
aws cloudformation deploy --template-file aws-web-app-output.yaml --stack-name myrydes-nate-baker --capabilities CAPABILITY_NAMED_IAM
```

[View your stack output](#stack-output) and update website/js/config.js with your environment-specific values:
  * userPoolId - from stack output
  * userPoolClientId - from stack output
  * region - your default region from stack output


Upload the static website files and your modified config.js to the S3 bucket:
```
aws s3 sync ./website s3://myrydes-nate-baker
```

## Validate Your Environment

Browse to the static website using the url found in your [stack output](#stack-output).

Register a new user to test your user pool configuration by following the [last step here](https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-2/).

Manually test your Lambda function at the AWS Console by following the [last step here](https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-3/).

Verify that the API gateway is properly surfacing your Lambda function to the web app by following the [last step here](https://aws.amazon.com/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/module-4/).

## Notes

### <a id="stack-output"></a>How to check your CloudFormation stack output.
You can view the Outputs in the AWS Console or use the command below:
```
aws cloudformation describe-stacks --stack-name myrydes-nate-baker
```

If you want to remove config.js from version control:
```
git update-index --assume-unchanged website/js/config.js
```

### Cleanup

Empty the S3 bucket (objects in the bucket will prevent stack deletion):
```
aws s3 rm s3://myrydes-nate-baker --recursive
```

Delete the stack.  This command returns really fast -- you can check the AWS Console -> CloudFormation stack events to follow progress and see whether the stack was fully deleted.
```
aws cloudformation delete-stack --stack-name myrydes-nate-baker
```

Remove the stack artifacts.  Do NOT run this command on your website s3 bucket or you won't be able to delete the stack:
```
aws s3 rb s3://temp-stack-artifacts-nate-baker --force
```
### Possible Next steps

Convert CF YAML template to AWS SAM.

Break up into multiple stacks as a practice exercise.

Create a matching TerraForm template.