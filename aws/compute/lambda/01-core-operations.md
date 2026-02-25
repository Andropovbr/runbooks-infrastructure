# AWS Lambda -- Core Operations Runbook

## Create Function (CLI)

``` bash
aws lambda create-function   --function-name my-function   --runtime python3.12   --role arn:aws:iam::<account-id>:role/lambda-execution-role   --handler app.lambda_handler   --zip-file fileb://function.zip
```

## Update Code

``` bash
aws lambda update-function-code   --function-name my-function   --zip-file fileb://function.zip
```

## Invoke Function (Test)

``` bash
aws lambda invoke   --function-name my-function   --payload '{}'   response.json
```

## List Functions

``` bash
aws lambda list-functions
```

## Delete Function

``` bash
aws lambda delete-function --function-name my-function
```

------------------------------------------------------------------------

## IAM Best Practice

Execution role must include: - logs:CreateLogGroup -
logs:CreateLogStream - logs:PutLogEvents

Never hardcode credentials.
