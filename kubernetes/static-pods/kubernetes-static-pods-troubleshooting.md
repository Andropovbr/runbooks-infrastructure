# Kubernetes Static Pods -- Troubleshooting Deep Dive

Generated on: 2026-02-24

------------------------------------------------------------------------

## Scenario 1: kube-apiserver down

Symptoms:

-   kubectl connection refused
-   Port 6443 not responding

Actions:

``` bash
sudo crictl ps -a | grep kube-apiserver
sudo crictl logs <container-id>
sudo journalctl -u kubelet -n 100 --no-pager
```

Check manifest file:

``` bash
sudo cat /etc/kubernetes/manifests/kube-apiserver.yaml
```

If corrupted, regenerate:

``` bash
sudo kubeadm init phase control-plane apiserver
```

------------------------------------------------------------------------

## Scenario 2: Static pod recreated automatically after deletion

Explanation:

Kubelet monitors manifest file. Deleting via kubectl is ineffective.

Proper removal:

``` bash
sudo rm <staticPodPath>/<pod>.yaml
```

------------------------------------------------------------------------

## Scenario 3: Static Pod not appearing in cluster

Check:

``` bash
sudo ls <staticPodPath>
sudo journalctl -u kubelet -n 50 --no-pager
```

Possible causes:

-   YAML syntax error
-   Invalid image
-   Incorrect indentation

------------------------------------------------------------------------

## Scenario 4: Static Pod stuck in CrashLoopBackOff

Inspect container logs:

``` bash
sudo crictl logs <container-id>
```

Common causes:

-   Wrong command
-   Missing volume mount
-   Image pull error

------------------------------------------------------------------------

## Diagnostic Commands Summary

``` bash
ps aux | grep kubelet
cat /var/lib/kubelet/config.yaml
sudo journalctl -u kubelet -n 50 --no-pager
sudo crictl ps -a
sudo crictl logs <container-id>
```

------------------------------------------------------------------------

End of Troubleshooting Guide
