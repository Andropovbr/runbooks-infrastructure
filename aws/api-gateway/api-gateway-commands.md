# API Gateway -- Commands Runbook

## List APIs

``` bash
aws apigateway get-rest-apis
aws apigatewayv2 get-apis
```

## Create HTTP API

``` bash
aws apigatewayv2 create-api   --name my-http-api   --protocol-type HTTP
```

## Create Integration (HTTP API → Lambda)

``` bash
aws apigatewayv2 create-integration   --api-id <api-id>   --integration-type AWS_PROXY   --integration-uri <lambda-arn>   --payload-format-version 2.0
```

## Create Route

``` bash
aws apigatewayv2 create-route   --api-id <api-id>   --route-key "GET /health"   --target integrations/<integration-id>
```

## Deploy Stage

``` bash
aws apigatewayv2 create-stage   --api-id <api-id>   --stage-name prod   --auto-deploy
```

## Enable Access Logs (HTTP API)

``` bash
aws apigatewayv2 update-stage   --api-id <api-id>   --stage-name prod   --access-log-settings DestinationArn=<log-group-arn>,Format='$context.requestId'
```

## Create VPC Link

``` bash
aws apigatewayv2 create-vpc-link   --name my-vpc-link   --subnet-ids subnet-123 subnet-456   --security-group-ids sg-123456
```

## Test Endpoint

``` bash
curl https://<api-id>.execute-api.<region>.amazonaws.com/prod/health
```
