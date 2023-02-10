# kubelab

A Kubernetes cluster using Vagrant/VirtualBox, configured with Ansible. The
cluster is based on Ubuntu 20.04 LTS (Focal Fossa).

Adapted from https://github.com/itwonderlab/ansible-vbox-vagrant-kubernetes

## Prerequisites

* Vagrant (tested with 2.3.4)
* VirtualBox (tested with 6.1.38)
* Ansible (tested with 2.13.7)

> Note: You may need to adjust the `IP_BASE` variable in `./Vagrantfile` based
  on the host network(s) in your VirtualBox set up.

## Usage

```shell
# Provision the cluster
time vagrant up
vagrant up  73.01s user 22.48s system 16% cpu 9:54.65 total

# Ansible writes the kubeconfig for the cluster to ./kubeconfig. Tell kubectl to
# use this config:
export KUBECONFIG=${PWD}/kubeconfig

# View the nodes of the cluster
kubectl get nodes

NAME     STATUS     ROLES           AGE     VERSION
master   Ready      control-plane   7m3s    v1.26.1
node1    Ready      <none>          3m45s   v1.26.1
node2    NotReady   <none>          41s     v1.26.1

# Destroy the cluster
vagrant destroy -f
```
