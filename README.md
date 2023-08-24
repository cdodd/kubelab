# kubelab

A local kubernetes cluster of the following shape:

- Kubernetes 1.28
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
vagrant up  68.20s user 35.50s system 14% cpu 11:38.24 total

# Ansible writes the kubeconfig for the cluster to ./kubeconfig. Tell kubectl to
# use this config:
export KUBECONFIG=${PWD}/kubeconfig

# View the nodes of the cluster
kubectl get nodes

NAME     STATUS   ROLES           AGE     VERSION
master   Ready    control-plane   8m33s   v1.28.0
node1    Ready    <none>          4m26s   v1.28.0
node2    Ready    <none>          77s     v1.28.0

# Destroy the cluster
vagrant destroy -f
```
