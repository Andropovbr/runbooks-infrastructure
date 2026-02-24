# API Gateway -- Troubleshooting Guide

## 401 Unauthorized

Possible causes: - Missing/invalid JWT token - IAM signature invalid -
Authorizer misconfigured

Actions: - Validate Authorization header - Check authorizer logs -
Confirm issuer/audience

------------------------------------------------------------------------

## 403 Forbidden

Possible causes: - Resource policy blocking - IAM deny - API key
required but missing

Actions: - Review resource policy - Validate IAM permissions - Confirm
API key usage plan

------------------------------------------------------------------------

## 404 Not Found

Possible causes: - Wrong stage - Route not deployed - Base path mapping
incorrect

Actions: - Confirm stage URL - Check route list - Verify custom domain
mapping

------------------------------------------------------------------------

## 429 Too Many Requests

Cause: - Throttling exceeded

Actions: - Check stage throttle settings - Review usage plan limits -
Implement retry with backoff

------------------------------------------------------------------------

## 5XX Errors

Common causes: - Backend timeout - Lambda error - Integration
misconfigured

Actions: - Check CloudWatch logs - Review integration latency metric -
Confirm backend health

------------------------------------------------------------------------

## High Latency

Possible causes: - Cold start (Lambda) - No caching enabled - Backend
slow response

Actions: - Enable caching (REST) - Optimize backend - Analyze CloudWatch
metrics
