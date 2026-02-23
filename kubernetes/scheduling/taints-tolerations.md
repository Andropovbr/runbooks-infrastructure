# Runbook: Taints and Tolerations

## Purpose
Use taints to repel Pods from nodes and tolerations to allow specific Pods to be scheduled there.

Mental model:
- **Taint on node**: "keep pods away"
- **Toleration on pod**: "I'm allowed to be here"

---

## Inspect Taints

```bash
kubectl describe node node01 | sed -n '/Taints:/,/Conditions:/p'
```

Or:

```bash
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.taints}{"\n"}{end}'
```

---

## Add a Taint to a Node

Example: repel all pods without toleration:

```bash
kubectl taint node node01 dedicated=team-a:NoSchedule
```

Verify:

```bash
kubectl describe node node01 | grep -i taints -A2
```

---

## Pod Toleration Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: team-a-pod
spec:
  tolerations:
    - key: dedicated
      operator: Equal
      value: team-a
      effect: NoSchedule
  containers:
    - name: nginx
      image: nginx:stable
```

Apply:

```bash
kubectl apply -f pod.yaml
```

---

## Common Scheduling Patterns

### Allow pods on control-plane nodes (labs)
Control-plane nodes are often tainted `NoSchedule`. A broad toleration:

```yaml
tolerations:
  - operator: Exists
```

---

## Troubleshooting

### Pod Pending due to taint
```bash
kubectl describe pod <pod> | sed -n '/Events:/,$p'
```

You will see something like:
- `had taint {dedicated: team-a}, that the pod didn't tolerate`

Fix:
- add toleration to the pod/deployment, or
- remove/adjust taint.

---

## Remove a Taint

```bash
kubectl taint node node01 dedicated=team-a:NoSchedule-
```
