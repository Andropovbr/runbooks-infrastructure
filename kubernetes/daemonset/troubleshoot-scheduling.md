# Runbook: DaemonSet Not Scheduling on All Nodes

## Scenario
A DaemonSet is not running on all nodes (e.g. `DESIRED > CURRENT`).

## Step 1 — Check DaemonSet Overview

```bash
kubectl get ds -n <namespace>
kubectl describe ds <name> -n <namespace>
```

Look for:
- `Desired Number of Nodes Scheduled`
- `Current Number of Nodes Scheduled`
- Events indicating scheduling failures

## Step 2 — Confirm Node Inventory

```bash
kubectl get nodes -o wide
```

If a node is `NotReady`, the DaemonSet pod may not be scheduled.

## Step 3 — Validate Node Labels (nodeSelector / affinity)

```bash
kubectl get nodes --show-labels
```

If the DaemonSet uses `nodeSelector` or `nodeAffinity`,
confirm required labels exist on nodes.

## Step 4 — Check Node Taints (control-plane is common)

```bash
kubectl describe node <node-name>
```

Look for taints such as:
- `NoSchedule`
- `NoExecute`

If nodes are tainted, ensure the DaemonSet tolerates them.
A common generic toleration:

```yaml
tolerations:
  - operator: Exists
```

## Step 5 — Describe a Pending Pod

```bash
kubectl get pods -n <namespace> -o wide
kubectl describe pod <pod-name> -n <namespace>
```

In **Events**, common messages:
- `node(s) didn't match node selector`
- `node(s) had taint {...} that the pod didn't tolerate`
- `Insufficient cpu`
- `Insufficient memory`

## Step 6 — Check Resource Pressure

```bash
kubectl describe node <node-name>
kubectl top node
kubectl top pod -A
```

If the node is resource constrained:
- reduce requests/limits, or
- add capacity (more nodes / larger nodes)
