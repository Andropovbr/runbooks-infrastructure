# Runbook: Pods – Deploy and Debug

## Create Pod

```bash
kubectl run nginx --image=nginx --restart=Never
```

## Apply Manifest

```bash
kubectl apply -f pod.yaml
```

## Inspect Pod

```bash
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

## Exec Into Pod

```bash
kubectl exec -it <pod-name> -- sh
```

## Common Issues

### CrashLoopBackOff
- Check logs
- Check command/args
- Check readiness/liveness probes

### ImagePullBackOff
- Validate image name
- Check private registry secret

### Pending
- Check node resources
- Check scheduling events
