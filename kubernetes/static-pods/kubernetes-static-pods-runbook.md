# Kubernetes Static Pods -- Operational Runbook

Generated on: 2026-02-24

------------------------------------------------------------------------

## 1. Concept Overview

A Static Pod is managed directly by the kubelet and defined by a local
manifest file on a node.

Key characteristics:

-   Not managed by the API Server
-   Not scheduled by the Scheduler
-   No ownerReferences
-   Created from a file defined by kubelet (staticPodPath)

------------------------------------------------------------------------

## 2. How to Identify Static Pods

### Check owner references

``` bash
kubectl get pod <pod-name> -o yaml | grep ownerReferences -A 5
```

If no ownerReferences → likely a Static Pod.

### Check kubelet config for manifest path

``` bash
cat /var/lib/kubelet/config.yaml | grep staticPodPath
```

or

``` bash
ps aux | grep kubelet | grep pod-manifest-path
```

------------------------------------------------------------------------

## 3. Create a Static Pod

1.  SSH into target node
2.  Create manifest file in staticPodPath

Example:

``` bash
sudo tee /etc/kubernetes/manifests/static-busybox.yaml <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: static-busybox
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sleep", "1000"]
EOF
```

------------------------------------------------------------------------

## 4. Modify a Static Pod

Edit manifest file directly:

``` bash
sudo vi /etc/kubernetes/manifests/static-busybox.yaml
```

Kubelet will automatically recreate the pod.

------------------------------------------------------------------------

## 5. Delete a Static Pod

Delete the manifest file:

``` bash
sudo rm /etc/kubernetes/manifests/static-busybox.yaml
```

Verify removal:

``` bash
kubectl get pods
```

------------------------------------------------------------------------

## 6. Troubleshooting Scenarios

### API Server connection refused

Check if kube-apiserver static pod is running:

``` bash
sudo crictl ps | grep kube-apiserver
```

Check kubelet logs:

``` bash
sudo journalctl -u kubelet -n 50 --no-pager
```

------------------------------------------------------------------------

### Static Pod not updating after manifest change

Force recreation:

``` bash
sudo mv /etc/kubernetes/manifests/pod.yaml /tmp/
sleep 5
sudo mv /tmp/pod.yaml /etc/kubernetes/manifests/
```

------------------------------------------------------------------------

### Static Pod directory unknown

Find staticPodPath:

``` bash
cat /var/lib/kubelet/config.yaml
```

------------------------------------------------------------------------

## 7. CKA Exam Checklist

-   Never use kubectl to create/delete static pods
-   Always operate on node filesystem
-   Verify staticPodPath
-   Check ownerReferences
-   Use crictl or journalctl for debugging

------------------------------------------------------------------------

End of Runbook
