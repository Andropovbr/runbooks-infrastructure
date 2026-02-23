# Runbook: Upgrade a DaemonSet Image

## Purpose
Update a DaemonSet image and safely monitor rollout, with rollback if needed.

## Update Image

```bash
kubectl set image ds/<name> <container-name>=<new-image> -n <namespace>
```

Example:

```bash
kubectl set image ds/elasticsearch fluentd=registry.k8s.io/fluentd-elasticsearch:1.21 -n kube-system
```

## Monitor Rollout

```bash
kubectl rollout status ds/<name> -n <namespace>
```

## Verify Pods

```bash
kubectl get pods -n <namespace> -o wide
kubectl describe ds <name> -n <namespace>
```

## Rollback

```bash
kubectl rollout undo ds/<name> -n <namespace>
```

## Force Restart (same image, redeploy pods)

```bash
kubectl rollout restart ds/<name> -n <namespace>
```
