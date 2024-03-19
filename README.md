# kubelab

A local kubernetes cluster of the following shape:

- Kubernetes 1.29
- VM host: Vagrant & VirtualBox
- OS: Ubuntu 22.04 LTS (Jammy Jellyfish)
- Cluster creation: kubeadm
- Provisioning: Ansible
- CNI: Calico

## Prerequisites

* Vagrant (tested with 2.4.1)
* VirtualBox (tested with 7.0.14)
* Ansible (tested with 2.16.4)
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
vagrant up  46.91s user 35.87s system 10% cpu 12:55.65 total

# Ansible writes the kubeconfig for the cluster to ./kubeconfig. Tell kubectl to
# use this config:
export KUBECONFIG=${PWD}/kubeconfig

# View the nodes of the cluster
kubectl get nodes

NAME     STATUS   ROLES           AGE     VERSION
master   Ready    control-plane   9m20s   v1.29.3
node1    Ready    <none>          4m34s   v1.29.3
node2    Ready    <none>          51s     v1.29.3

# Destroy the cluster
vagrant destroy -f
```
