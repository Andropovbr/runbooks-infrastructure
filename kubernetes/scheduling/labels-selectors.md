# Runbook: Labels and Selectors for Scheduling (nodeSelector)

## Purpose
Use node labels and `nodeSelector` to restrict Pods to specific nodes.

---

## Label a Node

Check current labels:

```bash
kubectl get nodes --show-labels
```

Add a label:

```bash
kubectl label node node01 color=blue
```

Verify:

```bash
kubectl get node node01 --show-labels
```

---

## Schedule a Pod Using nodeSelector

Example manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: blue-pod
spec:
  nodeSelector:
    color: blue
  containers:
    - name: nginx
      image: nginx:stable
```

Apply:

```bash
kubectl apply -f pod.yaml
```

Validate placement:

```bash
kubectl get pod blue-pod -o wide
```

---

## Troubleshooting

### Pod Pending (no matching nodes)
Check scheduler events:

```bash
kubectl describe pod blue-pod
```

Check labels on all nodes:

```bash
kubectl get nodes --show-labels | grep -E "NAME|color="
```

Fix by adding the label to the correct node:

```bash
kubectl label node <node> color=blue --overwrite
```

### Removing a label
```bash
kubectl label node node01 color-
```
