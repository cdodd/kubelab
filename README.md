# kubelab

A local kubernetes cluster of the following shape:

- Kubernetes 1.31
- VM host: Vagrant & VirtualBox
- OS: Ubuntu 24.04 LTS (Noble Numbat)
- Cluster creation: kubeadm
- Provisioning: Ansible
- CNI: Cilium

## Prerequisites

* Vagrant (tested with 2.4.1)
* VirtualBox (tested with 7.0.16)
* Ansible (tested with 2.17.5)

> The `kubernetes.core` collection is required for creating kubernetes resources
  and installing helm charts. It can be installed by running
  `ansible-galaxy collection install kubernetes.core`

## Usage

> *Note*: You may need to adjust the `IP_BASE` variable in `./Vagrantfile` based
  on the host network(s) in your VirtualBox set up.

```shell
# Provision the cluster
time vagrant up
vagrant up  45.04s user 22.07s system 12% cpu 8:46.65 total

# Ansible writes the kubeconfig for the cluster to ./kubeconfig. Tell kubectl to
# use this config:
export KUBECONFIG=${PWD}/kubeconfig

# View the nodes of the cluster
kubectl get nodes -o wide

NAME     STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION     CONTAINER-RUNTIME
master   Ready    control-plane   6m51s   v1.31.1   10.0.2.15     <none>        Ubuntu 24.04 LTS   6.8.0-31-generic   containerd://1.7.23
node1    Ready    <none>          3m50s   v1.31.1   10.0.2.15     <none>        Ubuntu 24.04 LTS   6.8.0-31-generic   containerd://1.7.23
node2    Ready    <none>          68s     v1.31.1   10.0.2.15     <none>        Ubuntu 24.04 LTS   6.8.0-31-generic   containerd://1.7.23

# Destroy the cluster
vagrant destroy -f
```
