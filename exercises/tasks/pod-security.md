Here’s an example task focused on configuring the API server and scheduler by enabling a specific admission controller in Kubernetes.

---

### Task: Enable the `PodSecurity` Admission Controller on the API Server

**Objective**: Modify the API server configuration to enable the `PodSecurity` admission controller, which enforces security policies on Pods based on predefined security standards. This will allow you to set specific security profiles (e.g., `privileged`, `baseline`, and `restricted`) on different namespaces in the cluster.

---

### Scenario

Your Kubernetes cluster hosts a variety of applications, some of which require higher security controls. To enforce stricter security policies for different namespaces, you need to enable the `PodSecurity` admission controller. This controller allows you to enforce Kubernetes Pod Security Standards at the namespace level, making it easier to prevent potentially unsafe workloads.

### Requirements

1. **Edit the API server configuration**: Enable the `PodSecurity` admission controller in the API server configuration.
2. **Apply security policies**: Once enabled, label namespaces with specific security profiles (e.g., `privileged`, `baseline`, `restricted`).
3. **Verify enforcement**: Confirm that the admission controller is enforcing the policies by attempting to create a Pod that violates a restricted profile.

---

### Steps

#### 1. Modify the API Server Configuration to Enable `PodSecurity`

The `PodSecurity` admission controller can typically be enabled by editing the API server configuration file (e.g., `kube-apiserver.yaml` on the control plane node). Follow these steps:

1. **SSH into the control plane node**:
   ```bash
   ssh <control-plane-node>
   ```

2. **Locate the API server configuration** (usually found in `/etc/kubernetes/manifests/kube-apiserver.yaml`).

3. **Edit the file** to add the `PodSecurity` admission controller to the list of enabled admission plugins. Open the file in a text editor:
   ```bash
   sudo nano /etc/kubernetes/manifests/kube-apiserver.yaml
   ```

4. **Add the `PodSecurity` admission controller** by locating the `--enable-admission-plugins` flag. Ensure that `PodSecurity` is included:
   ```yaml
   - --enable-admission-plugins=NodeRestriction,PodSecurity
   ```

5. **Save and exit** the file. The kubelet will automatically detect the change and restart the API server with the updated configuration.

#### 2. Label Namespaces with Pod Security Levels

With the `PodSecurity` admission controller enabled, you can now label namespaces to apply different security profiles.

1. **Label the `dev` namespace** as `restricted` (the most restrictive profile):
   ```bash
   kubectl label namespace dev pod-security.kubernetes.io/enforce=restricted
   ```

2. **Label the `test` namespace** as `baseline`:
   ```bash
   kubectl label namespace test pod-security.kubernetes.io/enforce=baseline
   ```

3. **Label the `prod` namespace** as `privileged` (least restrictive):
   ```bash
   kubectl label namespace prod pod-security.kubernetes.io/enforce=privileged
   ```

#### 3. Test the Pod Security Enforcement

To verify that the `PodSecurity` admission controller is enforcing policies as expected, attempt to create a Pod in the `dev` namespace (labeled `restricted`) that violates the restricted policy.

1. **Create a test Pod configuration file** (`restricted-violation.yaml`) with settings that will not comply with the `restricted` policy (e.g., a privileged container).

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: test-pod
     namespace: dev
   spec:
     containers:
     - name: test-container
       image: nginx
       securityContext:
         privileged: true  # This should trigger a violation in the restricted namespace
   ```

2. **Attempt to apply the Pod**:
   ```bash
   kubectl apply -f restricted-violation.yaml
   ```

3. **Check the output**. You should see an error indicating that the Pod violates the namespace’s `restricted` security policy.

   ```plaintext
   Error from server (Forbidden): error when creating "restricted-violation.yaml": admission webhook "PodSecurity" denied the request: privileged containers are not allowed in the restricted profile
   ```

### Verification

1. **Check that the API server is running with the correct admission controllers**:
   ```bash
   kubectl logs -n kube-system kube-apiserver-<node-name> | grep "admission"
   ```
   This should show that `PodSecurity` is enabled.

2. **Verify namespace labels**:
   ```bash
   kubectl get namespace --show-labels
   ```
   This should display the `pod-security.kubernetes.io/enforce` labels for each namespace.

### Conclusion

In this task, you enabled the `PodSecurity` admission controller, labeled namespaces with specific security levels, and verified enforcement by attempting to create a non-compliant Pod. This configuration helps ensure that Pods are created in compliance with security best practices based on the designated security profile for each namespace.
