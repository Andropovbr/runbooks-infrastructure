# Runbook: Node Affinity (requiredDuringSchedulingIgnoredDuringExecution)

## Purpose
Use node affinity for more expressive scheduling rules than `nodeSelector`.

Key types:
- `requiredDuringSchedulingIgnoredDuringExecution` (hard requirement)
- `preferredDuringSchedulingIgnoredDuringExecution` (soft preference)

This runbook focuses on **required** affinity (CKA-style).

---

## Example: Pin Pods to nodes with `color=blue`

Label the node (if needed):

```bash
kubectl label node node01 color=blue --overwrite
```

Deployment example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 2
  selector:
    matchLabels:
      app: blue
  template:
    metadata:
      labels:
        app: blue
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                  - key: color
                    operator: In
                    values:
                      - blue
      containers:
        - name: nginx
          image: nginx:stable
```

Apply:

```bash
kubectl apply -f deploy.yaml
```

Validate:

```bash
kubectl get pods -l app=blue -o wide
```

---

## Troubleshooting

### Pods Pending
Describe one pod:

```bash
kubectl describe pod <pod-name>
```

Check nodes and labels:

```bash
kubectl get nodes --show-labels | grep -E "NAME|color="
```

Common causes:
- Misspelled `matchExpressions`
- Using `value` instead of `values` (must be a list)
- Wrong operator (`Equals` is invalid; use `In`, `NotIn`, `Exists`, `DoesNotExist`, `Gt`, `Lt`)

---

## Quick Affinity Operators Cheat Sheet

- `In`: key must exist and be in list
- `NotIn`: key exists but not in list
- `Exists`: key exists (any value)
- `DoesNotExist`: key must not exist
- `Gt`, `Lt`: numeric comparisons (label value treated as number)
