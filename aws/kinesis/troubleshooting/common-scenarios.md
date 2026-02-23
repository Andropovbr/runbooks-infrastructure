# Kinesis Troubleshooting Scenarios

---

## 1. High Iterator Age (Consumer Lag)

### Symptoms
- CloudWatch metric: GetRecords.IteratorAgeMilliseconds increasing

### Causes
- Consumer too slow
- Not enough shards
- Processing bottleneck downstream

### Actions
- Increase shard count
- Scale consumer horizontally
- Optimize processing logic

---

## 2. ProvisionedThroughputExceededException

### Symptoms
- Errors when producing or consuming

### Causes
- Too many records per shard
- Hot partition key

### Actions
- Increase shard count
- Improve partition key distribution

---

## 3. Hot Shard

### Symptoms
- Uneven shard metrics
- One shard with high IncomingBytes

### Actions
- Redesign partition key
- Add randomness or hashing

---

## 4. Firehose Not Delivering to S3

### Symptoms
- DeliveryToS3.Success = 0

### Actions
- Check IAM role permissions
- Verify S3 bucket policy
- Check buffering configuration

---

## 5. Data Analytics Job Behind

### Symptoms
- MillisBehindLatest increasing

### Actions
- Scale parallelism
- Optimize SQL query
- Check input stream throughput
