# CKS Exam Overview

## 1. **Cluster Setup (10%)**
   - Set up Role-Based Access Control (RBAC) to restrict access to specific namespaces.
   - Enable and configure Kubernetes audit logging for cluster security auditing.
   - Use `kubeadm` to initialize a cluster with custom security configurations, such as a specific `PodSecurityPolicy`.

## 2. **Cluster Hardening (15%)**
   - Enable and configure Pod Security Policies (PSP) or enforce Pod Security Standards (PSS) for a namespace to restrict privileged containers.
   - Set network policies in a namespace to allow traffic only between specific pods.
   - Configure the Kubernetes API server to restrict anonymous access and enforce strict API access rules.
   - Disable unnecessary or insecure services on the nodes, such as setting up firewalls to allow only specific ports.
   - Use Linux security tools like `AppArmor` or `Seccomp` to restrict pod actions.

## 3. **System Hardening (15%)**
   - Harden the Linux operating system on Kubernetes nodes by ensuring the latest security patches are applied.
   - Implement kernel hardening measures, such as configuring `Sysctl` parameters for network security.
   - Limit container capabilities to reduce the scope of potential threats.
   - Set up file system policies to restrict pod access to specific host paths.
   - Configure logging for Docker or the container runtime to capture and monitor container activity.

## 4. **Minimize Microservices Vulnerabilities (20%)**
   - Scan container images for known vulnerabilities using tools like `Trivy` or `Clair`.
   - Create a `NetworkPolicy` to isolate sensitive microservices from other parts of the application.
   - Ensure pods are configured to run with non-root users by setting a security context in the deployment specs.
   - Restrict containers from accessing the Kubernetes API server unless necessary.

## 5. **Supply Chain Security (20%)**
   - Integrate container image scanning results with vulnerability management systems.
   - Set up and enforce policies to pull images only from a private, trusted registry.

## 6. **Monitoring, Logging, and Runtime Security (20%)**
   - Configure `Falco` or `Sysdig` to monitor runtime behavior of containers and detect anomalies.
   - Use Kubernetes audit logs to track security events like failed authentication attempts.
