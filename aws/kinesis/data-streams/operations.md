# Kinesis Data Streams — Operations

## List Streams
aws kinesis list-streams

## Describe Stream
aws kinesis describe-stream --stream-name <stream-name>

## Enable Enhanced Monitoring
aws kinesis enable-enhanced-monitoring   --stream-name <stream-name>   --shard-level-metrics IncomingBytes OutgoingBytes

## Update Shard Count (Scaling)
aws kinesis update-shard-count   --stream-name <stream-name>   --target-shard-count <number>   --scaling-type UNIFORM_SCALING

## Increase Retention Period
aws kinesis increase-stream-retention-period   --stream-name <stream-name>   --retention-period-hours 48

## Decrease Retention Period
aws kinesis decrease-stream-retention-period   --stream-name <stream-name>   --retention-period-hours 24

## Get Shard Iterator
aws kinesis get-shard-iterator   --stream-name <stream-name>   --shard-id shardId-000000000000   --shard-iterator-type TRIM_HORIZON

## Read Records
aws kinesis get-records   --shard-iterator <iterator>
