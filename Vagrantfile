# IMAGE_NAME = "bento/ubuntu-20.04"
IMAGE_NAME = "ubuntu/focal64"
MASTERS_NUM = 1
MASTERS_CPU = 2
MASTERS_MEM = 2048

NODES_NUM = 2
NODES_CPU = 2
NODES_MEM = 2048

IP_BASE = "192.168.50."

VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    (1..MASTERS_NUM).each do |i|
        config.vm.define "k8s-m-#{i}" do |master|
            master.vm.box = IMAGE_NAME
            master.vm.network "private_network", ip: "#{IP_BASE}#{i + 10}"
            master.vm.hostname = "k8s-m-#{i}"
            master.vm.provider "virtualbox" do |v|
                v.memory = MASTERS_MEM
                v.cpus = MASTERS_CPU
            end
            master.vm.provision "ansible" do |ansible|
                ansible.playbook = "roles/kubernetes.yml"
                ansible.compatibility_mode = "2.0"
                ansible.extra_vars = {
                    master_admin_user:  "vagrant",
                    master_admin_group: "vagrant",
                    master_apiserver_advertise_address: "#{IP_BASE}#{i + 10}",
                    node_public_ip: "#{IP_BASE}#{i + 10}"
                }
            end
        end
    end

    (1..NODES_NUM).each do |j|
        config.vm.define "k8s-n-#{j}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "#{IP_BASE}#{j + 10 + MASTERS_NUM}"
            node.vm.hostname = "k8s-n-#{j}"
            node.vm.provider "virtualbox" do |v|
                v.memory = NODES_MEM
                v.cpus = NODES_CPU
            end
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "roles/kubernetes.yml"
                ansible.compatibility_mode = "2.0"
                ansible.extra_vars = {
                    node_admin_user: "vagrant",
                    node_admin_group: "vagrant",
                    node_public_ip: "#{IP_BASE}#{j + 10 + MASTERS_NUM}"
                }
            end
        end
    end
end
