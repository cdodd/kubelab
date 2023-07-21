# kubelab

A local kubernetes cluster of the following shape:

- Kubernetes 1.27.4
- VM host: Vagrant & VirtualBox
- OS: Ubuntu 22.04 LTS (Jammy Jellyfish)
- Cluster creation: kubeadm
- Provisioning: Ansible
- CNI: Cilium (via Helm chart)

## Prerequisites

* Vagrant (tested with 2.3.4)
* VirtualBox (tested with 6.1.38)
* Ansible (tested with 2.13.7)
  * The `kubernetes.core` collection is required for creating kubernetes
    resources and installing helm charts.

> The `kubernetes.core` collection can be installed by running
  `ansible-galaxy collection install kubernetes.core`

## Usage

> *Note*: You may need to adjust the `IP_BASE` variable in `./Vagrantfile` based
  on the host network(s) in your VirtualBox set up.

```shell
# Provision the cluster
time vagrant up
vagrant up  207.45s user 46.13s system 25% cpu 16:47.18 total

# Ansible writes the kubeconfig for the cluster to ./kubeconfig. Tell kubectl to
# use this config:
export KUBECONFIG=${PWD}/kubeconfig

# View the nodes of the cluster
kubectl get nodes

NAME     STATUS   ROLES           AGE   VERSION
master   Ready    control-plane   12m   v1.27.4
node1    Ready    <none>          6m    v1.27.4
node2    Ready    <none>          57s   v1.27.4

# Destroy the cluster
vagrant destroy -f
```
