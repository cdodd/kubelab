# Cluster Upgrade: Solution

[< Back to exercise list](../README.md)

[< Back to upgrade-cluster-01.md](./upgrade-cluster-01.md)

1. Log in to the control plane node (`vagrant ssh master`) and perform the
   following steps:
   - Check available package versions:
     ```bash
     apt-cache policy kubeadm

     kubeadm:
     Installed: 1.30.1-1.1
     Candidate: 1.30.6-1.1
     Version table:
         1.30.6-1.1 500
             500 https://pkgs.k8s.io/core:/stable:/v1.30/deb  Packages
         1.30.5-1.1 500
             500 https://pkgs.k8s.io/core:/stable:/v1.30/deb  Packages
         [...]
     ```
   - Upgrade `kubeadm` to version `1.30.6`:
     ```bash
     sudo apt-get update && sudo apt-get install -y kubeadm=1.30.6-1.1
     ```
   - Verify the upgrade plan:
     ```bash
     sudo kubeadm upgrade plan
     ```
   - Execute the upgrade:
     ```bash
     sudo kubeadm upgrade apply v1.30.6
     ```

2. **Upgrade `kubelet` and `kubectl` on the control plane node**:
   ```bash
   sudo apt-get install -y kubelet=1.30.6-1.1 kubectl=1.30.6-1.1
   sudo systemctl restart kubelet
   ```

3. **Upgrade each worker node**:
   - For each worker node:
     - Drain the node:
       ```bash
       kubectl drain <node-name> --ignore-daemonsets
       ```
     - SSH into the node `vagrant ssh node[1|2]`
     - Upgrade `kubeadm`:
       ```bash
       sudo apt-get update && sudo apt-get install -y kubeadm=1.30.6-1.1
       sudo kubeadm upgrade node
       ```
     - Upgrade `kubelet` and `kubectl`:
       ```bash
       sudo apt-get install -y kubelet=1.30.6-1.1 kubectl=1.30.6-1.1
       sudo systemctl restart kubelet
       ```
     - Uncordon the node:
       ```bash
       kubectl uncordon <node-name>
       ```

4. **Verify the Upgrade**:
   - On any node, confirm all nodes are on the upgraded version:
     ```bash
     kubectl get nodes -o wide
     ```
   - Ensure all nodes show `1.30.6` under the `VERSION` column.
