# Runbook: Services – Connectivity & Debug

## Create ClusterIP Service

```bash
kubectl expose deploy nginx --port=80
```

## Inspect Service

```bash
kubectl get svc
kubectl describe svc <name>
```

## Check Endpoints

```bash
kubectl get endpoints <service-name>
```

## Test Connectivity

From inside cluster:

```bash
kubectl run tmp --image=busybox -it --rm -- sh
wget -qO- http://<service-name>
```

## Common Issues

### No Endpoints
- Label mismatch between Service selector and Pods

### Service Not Reachable
- Check NetworkPolicy
- Check kube-proxy
