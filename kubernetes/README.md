# Kubernetes Runbooks

This folder contains day-to-day operational runbooks and troubleshooting playbooks.

## Structure

- `daemonset/` — DaemonSet operations and troubleshooting
- (future) `deployment/` — Deployments, rollouts, scaling, probes
- (future) `service/` — Service reachability, endpoints, kube-proxy
- (future) `networking/` — DNS, NetworkPolicy, ingress
- (future) `storage/` — PV/PVC provisioning and attachment issues
- (future) `security/` — RBAC, service accounts, secrets

## Conventions

- Commands should be copy/paste ready.
- Use placeholders like `<namespace>` and `<name>` consistently.
- Prefer objective checks (`get`, `describe`, `events`, `top`) before changes.
