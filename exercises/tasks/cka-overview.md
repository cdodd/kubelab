# CKA Exam Topics

### 1. **Cluster Architecture, Installation, and Configuration** (25%)

- **Set up a Kubernetes cluster**: Install Kubernetes on multiple nodes using
  `kubeadm`, set up the control plane, and join worker nodes.
- **Configure etcd**: Backup and restore etcd, the key-value store for
  Kubernetes, using snapshots and `etcdctl`.
- **Manage certificates**: Use OpenSSL to view, renew, or replace cluster
  certificates.
- **Configure API server and scheduler**: Adjust API server or scheduler
  settings, such as enabling specific admission controllers.
- **Set up network policies**: Define network policies to restrict or allow
  traffic between pods and namespaces.

### 2. **Workloads and Scheduling** (15%)

- **Create and manage Pods**: Create a Pod using a YAML file, update a running
  Pod, and configure it with specific resource limits.
- **Manage deployments**: Create a deployment, scale it, and roll out updates or
  rollbacks as needed.
- **Work with jobs and cron jobs**: Create a job or cron job, monitor its
  execution, and ensure it runs to completion or on schedule.
- **Configure pod scheduling constraints**: Use node selectors, affinities, and
  tolerations to control Pod placement.
- **Manage DaemonSets**: Create and update a DaemonSet to ensure a Pod runs on
  all (or specific) nodes.

### 3. **Services and Networking** (20%)

- **Expose applications**: Create a Service to expose a Pod internally using
  ClusterIP, or externally using NodePort or LoadBalancer.
- **Set up Ingress**: Configure an Ingress resource to manage HTTP traffic for
  multiple services.
- **Troubleshoot networking issues**: Diagnose issues with pod-to-pod and
  pod-to-service communication.
- **Manage CoreDNS**: Configure DNS for a Kubernetes cluster to resolve service
  names and troubleshoot DNS-related issues.
- **Implement Network Policies**: Define network policies to control the
  communication between namespaces or pods.

### 4. **Storage** (10%)

- **Provision persistent volumes**: Create and use PersistentVolumes (PVs) and
  PersistentVolumeClaims (PVCs) for stateful applications.
- **Configure storage classes**: Define and use different StorageClasses for
  dynamic provisioning of PersistentVolumes.
- **Manage volume claims in pods**: Attach PVCs to Pods and troubleshoot
  mounting issues.
- **Use ephemeral volumes**: Attach emptyDir volumes to Pods and configure them
  for temporary storage.

### 5. **Troubleshooting** (30%)

- **Troubleshoot node and pod issues**: Diagnose issues like node failures, pod
  crashes, or resource exhaustion.
- **Work with logs**: Use `kubectl logs` and `kubectl describe` to troubleshoot
  Pods and services.
- **Inspect resource configurations**: Check and fix misconfigured resources
  like ConfigMaps, Secrets, and Service Accounts.
- **Identify issues with deployments and jobs**: Diagnose issues with deployment
  rollouts, job completions, and cron job schedules.
- **Diagnose networking issues**: Use `kubectl get events` and
  `kubectl describe` to troubleshoot networking problems.
