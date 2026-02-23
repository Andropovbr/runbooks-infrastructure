# Runbook: Manual Scheduling (nodeName)

## Purpose
Force a Pod to run on a specific node using `spec.nodeName`.
This is useful for labs and controlled scenarios.

> Note: In production, prefer `nodeSelector`, `affinity`, and `taints/tolerations` instead of hard pinning.

---

## Identify Candidate Nodes

```bash
kubectl get nodes -o wide
```

---

## Create a Pod Manifest (Example)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: manual-pod
spec:
  nodeName: node01
  containers:
    - name: nginx
      image: nginx:stable
```

Apply:

```bash
kubectl apply -f pod.yaml
```

---

## Validate Placement

```bash
kubectl get pod manual-pod -o wide
kubectl describe pod manual-pod
```

Look for:
- `Node:` set to the desired node
- Events showing successful scheduling

---

## Troubleshooting

### Pod stays Pending
`nodeName` bypasses the scheduler, but the kubelet can still reject the pod if:
- Node is NotReady
- Insufficient resources (CPU/memory)
- Volume/node constraints

Check:

```bash
kubectl describe pod manual-pod
kubectl describe node node01
kubectl get events --sort-by=.lastTimestamp | tail -n 30
```

### Move the Pod to another node
Delete and recreate with a different `nodeName`:

```bash
kubectl delete pod manual-pod
# edit pod.yaml to nodeName: node02
kubectl apply -f pod.yaml
```
