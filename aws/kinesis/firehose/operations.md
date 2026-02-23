# Kinesis Data Firehose — Operations

## List Delivery Streams
aws firehose list-delivery-streams

## Describe Delivery Stream
aws firehose describe-delivery-stream   --delivery-stream-name <name>

## Update Destination
aws firehose update-destination   --delivery-stream-name <name>   --current-delivery-stream-version-id <version-id>

## Common Destinations
- Amazon S3
- Amazon Redshift
- Amazon OpenSearch
- HTTP Endpoint

## Check Delivery Errors (CloudWatch)
Look for metrics:
- DeliveryToS3.Success
- DeliveryToS3.DataFreshness
- DeliveryToS3.Records
