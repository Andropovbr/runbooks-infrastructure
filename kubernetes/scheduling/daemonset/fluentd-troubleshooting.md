# Runbook: Fluentd DaemonSet Troubleshooting

## Scenario
Fluentd is deployed as a DaemonSet, but logs are not reaching Elasticsearch (or another backend).

## Step 1 — Confirm DaemonSet and Pods

```bash
kubectl -n kube-system get ds
kubectl -n kube-system get pods -o wide
```

If you used a label like `app=elasticsearch`:

```bash
kubectl -n kube-system get pods -l app=elasticsearch -o wide
```

## Step 2 — Check Container Logs

```bash
kubectl -n kube-system logs <pod-name>
```

Look for:
- connection refused / timeout
- DNS resolution failures
- authentication/authorization errors
- output plugin configuration errors

## Step 3 — Describe Pod (events + mounts)

```bash
kubectl -n kube-system describe pod <pod-name>
```

Check:
- Events (pull errors, restarts, scheduling)
- Volume mounts (e.g. `/var/log`, container logs path)

## Step 4 — Validate Backend Connectivity from Pod

```bash
kubectl -n kube-system exec -it <pod-name> -- sh
```

DNS test:

```sh
nslookup elasticsearch
```

HTTP test (example):

```sh
wget -qO- http://elasticsearch:9200
```

If DNS fails:
- check CoreDNS
- check Service name / namespace
- check NetworkPolicy (if used)

## Step 5 — Resource Usage / OOM

```bash
kubectl top pod -n kube-system
kubectl top node
```

If the container is being killed (`OOMKilled` in `describe pod`):
- increase memory requests/limits
- reduce log volume / add filtering

## Step 6 — Host Log Availability (when using hostPath)

If Fluentd relies on hostPath volumes, confirm the paths exist on nodes:
- `/var/log`
- container logs directory (Docker vs containerd differs)

Note:
- Docker often uses `/var/lib/docker/containers`
- containerd commonly uses `/var/log/pods` and `/var/log/containers`
