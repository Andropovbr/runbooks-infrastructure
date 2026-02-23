# Runbook: Deploy and Validate a DaemonSet

## Purpose
Deploy a DaemonSet and validate that **one pod** is running per **eligible node**.

## Deploy

```bash
kubectl apply -f daemonset.yaml
```

## Validate DaemonSet Status

```bash
kubectl get ds -n <namespace>
```

Expected:
- `DESIRED` = number of eligible nodes
- `CURRENT` = same as `DESIRED`
- `READY` = same as `DESIRED`

## Validate Pod Distribution

```bash
kubectl get pods -n <namespace> -o wide
```

Confirm:
- Exactly **one pod per eligible node**
- No `CrashLoopBackOff`
- No `ImagePullBackOff`

## Inspect DaemonSet Details

```bash
kubectl describe ds <name> -n <namespace>
```

Check:
- Events
- Node selectors / affinity
- Tolerations
- Image and tag
- Update strategy

## Watch Rollout Status

```bash
kubectl rollout status ds/<name> -n <namespace>
```

## Force Restart (Redeploy Pods)

```bash
kubectl rollout restart ds/<name> -n <namespace>
```
