BOX_NAME = "bento/ubuntu-24.04" # (Noble Numbat)

MASTER_CPU = 2
MASTER_MEM = 2048

NODE_COUNT = 2
NODE_CPU = 2
NODE_MEM = 2048

# This may need to be updated based on the host network(s) in your VirtualBox set up
IP_BASE = "192.168.56"

Vagrant.configure("2") do |config|
    config.vm.define "master" do |master|
        master.vm.box = BOX_NAME
        master.vm.network "private_network", ip: "#{IP_BASE}.10"
        master.vm.hostname = "master"
        master.vm.synced_folder ".", "/vagrant", disabled: true
        master.vm.provider "virtualbox" do |v|
            v.memory = MASTER_MEM
            v.cpus = MASTER_CPU
        end
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible-config/playbook.yaml"
            ansible.compatibility_mode = "2.0"
            ansible.extra_vars = {
                public_ip: "#{IP_BASE}.10",
                ansible_python_interpreter: "python3"
            }
        end
    end

    (1..NODE_COUNT).each do |i|
        config.vm.define "node#{i}" do |node|
            node.vm.box = BOX_NAME
            node.vm.network "private_network", ip: "#{IP_BASE}.#{10 + i}"
            node.vm.hostname = "node#{i}"
            node.vm.synced_folder ".", "/vagrant", disabled: true
            node.vm.provider "virtualbox" do |v|
                v.memory = NODE_MEM
                v.cpus = NODE_CPU
            end
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible-config/playbook.yaml"
                ansible.compatibility_mode = "2.0"
                ansible.extra_vars = {
                    public_ip: "#{IP_BASE}.#{10 + i}",
                    ansible_python_interpreter: "python3"
                }
            end
        end
    end
end
