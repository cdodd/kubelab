# Configure and Use a Local Storage Class

1. **Create a StorageClass**:
    - Name the `StorageClass` **`local-storage`**.
    - The storage class should provision storage using the `kubernetes.io/no-provisioner` provisioner, which is suitable for local storage.

2. **Configure a PersistentVolume (PV)**:
    - Manually create a PV that uses the `local-storage` StorageClass.
    - Ensure that the PV points to a local directory on the host (e.g., `/mnt/data`).
    - Set the capacity of the PV to **1Gi**.
    - Restrict the PV to using `node1`

3. **Create a PersistentVolumeClaim (PVC)**:
    - Create a PVC that requests 1Gi of storage using the `local-storage` StorageClass.

4. **Test**:
    - Deploy a simple Pod that mounts the PVC, and create a file in the mounted directory to verify that the storage is working correctly.

## Docs

https://kubernetes.io/docs/concepts/storage/persistent-volumes/
