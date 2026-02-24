# API Gateway -- Common Procedures

## Expose Lambda as Public API

1.  Create Lambda function
2.  Create HTTP API
3.  Create AWS_PROXY integration
4.  Create route (e.g., GET /)
5.  Deploy stage
6.  Test with curl

------------------------------------------------------------------------

## Attach Custom Domain

1.  Request ACM certificate (same region as API)
2.  Create custom domain in API Gateway
3.  Map API + Stage
4.  Create Route 53 record (Alias → API Gateway)

------------------------------------------------------------------------

## Enable JWT Authorizer

1.  Configure OIDC/Cognito issuer
2.  Create JWT authorizer
3.  Attach authorizer to routes
4.  Test with valid Bearer token

------------------------------------------------------------------------

## Make API Private (REST API)

1.  Set endpoint type to PRIVATE
2.  Create Interface VPC Endpoint (execute-api)
3.  Attach Resource Policy restricting access to VPC endpoint
4.  Test from EC2 inside VPC

------------------------------------------------------------------------

## Enable Throttling

Stage level: - Configure burst + rate limit

Usage plan (REST): - Create API key - Attach usage plan - Set quota and
throttle
