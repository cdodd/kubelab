### Provision Persistent Storage for a MySQL Database

**Objective**: Create and use a PersistentVolume (PV) and PersistentVolumeClaim
(PVC) for a MySQL Pod. The database requires persistent storage to retain data
across Pod restarts and deployments.

### Requirements

1. **Create a PersistentVolume (PV)**:
   - The volume should have at least **5Gi** of storage.
   - It should support **ReadWriteOnce** access mode.
   - Use the `manual` storage provisioning method.

2. **Create a PersistentVolumeClaim (PVC)**:
   - The claim should request **5Gi** of storage.
   - It should match the `ReadWriteOnce` access mode to bind to the
     PersistentVolume created in Step 1.

3. **Deploy a MySQL Pod**:
   - Use the MySQL image `mysql`.
   - Configure the Pod to mount the PVC to `/var/lib/mysql`.
   - Set the MySQL root password to `password123` using an environment variable
     from a secret.

4. **Verify the Setup**:
   - Confirm that the Pod is running and that the PVC has successfully bound to
     the PV.
   - Check that the volume is mounted in the correct directory.

---

### Steps to Complete the Task

#### 1. Create the PersistentVolume (PV)

Create a file called `mysql-pv.yaml` with the following content:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/mysql  # Example path on the host node
```

Apply the PV definition:

```bash
kubectl apply -f mysql-pv.yaml
```

#### 2. Create the PersistentVolumeClaim (PVC)

Create a file called `mysql-pvc.yaml` with the following content:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

Apply the PVC definition:

```bash
kubectl apply -f mysql-pvc.yaml
```

#### 3. Deploy the MySQL Pod

Create a file called `mysql-pod.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mysql
spec:
  containers:
  - name: mysql
    image: mysql:5.7
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "password123"
    volumeMounts:
    - mountPath: "/var/lib/mysql"
      name: mysql-storage
  volumes:
  - name: mysql-storage
    persistentVolumeClaim:
      claimName: mysql-pvc
```

Apply the MySQL Pod definition:

```bash
kubectl apply -f mysql-pod.yaml
```

#### 4. Verify the Setup

1. **Check that the PV and PVC are bound**:
   ```bash
   kubectl get pv mysql-pv
   kubectl get pvc mysql-pvc
   ```

   Both should show a `STATUS` of `Bound`.

2. **Verify that the MySQL Pod is running**:
   ```bash
   kubectl get pod mysql
   ```

   The Pod should be in the `Running` state.

3. **Inspect the Pod’s volume mount**:
   ```bash
   kubectl exec mysql -- df -h /var/lib/mysql
   ```

   This should show that the volume is mounted correctly at `/var/lib/mysql`.

---

### Expected Outcome

- The `PersistentVolume` (PV) and `PersistentVolumeClaim` (PVC) are correctly
configured and bound.
- The MySQL Pod has successfully mounted the PVC at `/var/lib/mysql`.
- The MySQL container can store data persistently in the specified directory,
even if the Pod restarts.

This completes the task and ensures that a stateful application (MySQL) is
properly using persistent storage in Kubernetes.
