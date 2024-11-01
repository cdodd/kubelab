# Cluster Upgrade

Upgrade the Kubernetes control plane and worker nodes from version `1.30.1` to
version `1.30.6`.

**Requirements**:
1. Upgrade `kubeadm`, `kubelet`, and `kubectl` on all nodes (both control plane
   and worker nodes).
2. Make sure to drain and uncordon nodes as required during the upgrade process.
3. Confirm that all nodes are running the new version after the upgrade.

## Documentation

* https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

## Solution

See [upgrade-cluster-01-solution.md](./upgrade-cluster-01-solution.md).
