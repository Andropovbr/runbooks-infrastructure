# Runbook: Deployments – Rollout & Troubleshooting

## Create Deployment

```bash
kubectl create deployment nginx --image=nginx
```

## Inspect Deployment

```bash
kubectl get deploy
kubectl describe deploy <name>
```

## Rollout Status

```bash
kubectl rollout status deploy/<name>
```

## Update Image

```bash
kubectl set image deploy/<name> <container>=nginx:1.25
```

## Rollback

```bash
kubectl rollout undo deploy/<name>
```

## Common Issues

### Rollout Stuck
- Check events
- Check pod readiness probes

### Pods CrashLooping
- Inspect logs
- Validate environment variables
