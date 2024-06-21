# kubelab

A local kubernetes cluster of the following shape:

- Kubernetes 1.30
- VM host: Vagrant & VirtualBox
- OS: Ubuntu 24.04 LTS (Noble Numbat)
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
vagrant up  34.36s user 24.78s system 10% cpu 9:25.71 total

# Ansible writes the kubeconfig for the cluster to ./kubeconfig. Tell kubectl to
# use this config:
export KUBECONFIG=${PWD}/kubeconfig

# View the nodes of the cluster
kubectl get nodes -o wide

NAME     STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION     CONTAINER-RUNTIME
master   Ready    control-plane   6m45s   v1.30.2   10.0.2.15     <none>        Ubuntu 24.04 LTS   6.8.0-31-generic   containerd://1.7.18
node1    Ready    <none>          3m33s   v1.30.2   10.0.2.15     <none>        Ubuntu 24.04 LTS   6.8.0-31-generic   containerd://1.7.18
node2    Ready    <none>          42s     v1.30.2   10.0.2.15     <none>        Ubuntu 24.04 LTS   6.8.0-31-generic   containerd://1.7.18

# Destroy the cluster
vagrant destroy -f
```
