# Ansible VirtualBox Vagrant Kubernetes Lab

A kubernetes cluster using vagrant/virtualbox, configured with ansible. The
cluster is based on the official Ubuntu 20.04 LTS (focal) vagrant box.

Adapted from https://github.com/itwonderlab/ansible-vbox-vagrant-kubernetes

### Prerequisites

* Vagrant (tested with 2.2.6)
* VirtualBox (tested with 6.1)
* Ansible (tested with 2.12.1)

### Note

The variable `IP_BASE` in `./Vagrantfile` may need to be changed for different
operating systems. This worked for me:

```ruby
# MacOS
IP_BASE = "192.168.56."

# Linux
IP_BASE = "192.168.50."
```

### Usage

```shell
# Bring up the cluster
vagrant up

# Ansible writes the kubeconfig for the cluster to ./roles/kubeconfig. Tell
# kubectl to use this config:
export KUBECONFIG=${PWD}/roles/kubeconfig

# View the nodes of the cluster
kubectl get nodes

# Destroy the cluster
vagrant destroy
```
