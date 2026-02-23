# Runbook: Namespaces – Management & Isolation

## Create Namespace

```bash
kubectl create namespace dev
```

## List Namespaces

```bash
kubectl get ns
```

## Deploy to Specific Namespace

```bash
kubectl apply -f app.yaml -n dev
```

## Set Default Namespace

```bash
kubectl config set-context --current --namespace=dev
```

## Common Issues

### Resource Not Found
- Verify namespace context

### Stuck Terminating Namespace
- Check finalizers:
```bash
kubectl get ns <name> -o json
```
