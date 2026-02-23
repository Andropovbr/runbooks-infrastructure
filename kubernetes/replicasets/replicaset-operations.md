# Runbook: ReplicaSets – Operations & Troubleshooting

## Create ReplicaSet

```bash
kubectl apply -f replicaset.yaml
```

## Inspect ReplicaSet

```bash
kubectl get rs
kubectl describe rs <name>
```

## Scale ReplicaSet

```bash
kubectl scale rs <name> --replicas=5
```

## Common Issues

### Pods Not Matching Selector
- Ensure spec.selector matches pod template labels

### Insufficient Replicas
- Check node resources
- Check events
