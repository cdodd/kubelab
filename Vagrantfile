BOX_NAME = "bento/ubuntu-24.04" # (Noble Numbat)

MASTER_CPU = 2
MASTER_MEM = 2048

NODES_NUM = 2
NODES_CPU = 2
NODES_MEM = 2048

# This may need to be updated based on the host network(s) in your VirtualBox set up
IP_BASE = "192.168.56."

# Docs: https://developer.hashicorp.com/vagrant/docs/other/environmental-variables#vagrant_disable_vboxsymlinkcreate
VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.define "master" do |master|
        master.vm.box = BOX_NAME
        master.vm.network "private_network", ip: "#{IP_BASE}10"
        master.vm.hostname = "master"
        master.vm.provider "virtualbox" do |v|
            v.memory = MASTER_MEM
            v.cpus = MASTER_CPU
        end
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible-config/playbook.yaml"
            ansible.compatibility_mode = "2.0"
            ansible.extra_vars = {
                # kubernetes_version: "1.30.2-1.1", # Uncomment to override k8s version
                public_ip: "#{IP_BASE}10",
            }
        end
    end

    (1..NODES_NUM).each do |i|
        config.vm.define "node#{i}" do |node|
            node.vm.box = BOX_NAME
            node.vm.network "private_network", ip: "#{IP_BASE}#{10 + i}"
            node.vm.hostname = "node#{i}"
            node.vm.provider "virtualbox" do |v|
                v.memory = NODES_MEM
                v.cpus = NODES_CPU
            end
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible-config/playbook.yaml"
                ansible.compatibility_mode = "2.0"
                ansible.extra_vars = {
                    # kubernetes_version: "1.30.2-1.1", # Uncomment to override k8s version
                    public_ip: "#{IP_BASE}#{10 + i}",
                }
            end
        end
    end
end
