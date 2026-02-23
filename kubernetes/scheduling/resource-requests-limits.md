# Runbook: Resource Requests and Limits (CPU/Memory)

## Purpose
Control scheduling and runtime resource usage with requests and limits.

Mental model:
- **requests** decide scheduling placement (reserved for the pod)
- **limits** enforce runtime ceilings (what the pod cannot exceed)

---

## Quick Examples

### Pod with requests and limits

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resources-pod
spec:
  containers:
    - name: app
      image: nginx:stable
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "500m"
          memory: "256Mi"
```

Apply:

```bash
kubectl apply -f pod.yaml
```

---

## Inspect Resource Settings

```bash
kubectl get pod resources-pod -o jsonpath='{.spec.containers[0].resources}'
echo
kubectl describe pod resources-pod
```

---

## Troubleshooting

### Pod Pending: Insufficient cpu/memory
Check events:

```bash
kubectl describe pod resources-pod | sed -n '/Events:/,$p'
```

Check node allocatable and current allocations:

```bash
kubectl describe node <node-name> | sed -n '/Allocatable:/,/Allocated resources:/p'
```

Fix options:
- reduce requests
- add nodes / increase node size
- remove other workloads

### OOMKilled
Check last state:

```bash
kubectl describe pod resources-pod | grep -A5 -i "Last State"
```

Fix:
- increase memory limit, or
- reduce app memory usage

### CPU throttling (app slow)
CPU limits can throttle. Inspect metrics (if metrics-server installed):

```bash
kubectl top pod resources-pod
```

Fix:
- increase CPU limit, or
- remove limit (keep request only) depending on policy

---

## Common Daily Commands

```bash
kubectl top node
kubectl top pod -A
kubectl get pod -o wide
kubectl describe pod <pod>
kubectl get events --sort-by=.lastTimestamp | tail -n 50
```
