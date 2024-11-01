# SOLUTION: Configure and Use a Local Storage Class

1. **Create the StorageClass**:
    ```yaml
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: local-storage
    provisioner: kubernetes.io/no-provisioner
    volumeBindingMode: WaitForFirstConsumer
    ```

2. **Create the PersistentVolume (PV)**:
    Create a directory on `node1` to act as storage (e.g., `/mnt/data`):
    ```sh
    sudo mkdir -p /mnt/data
    sudo chmod 777 /mnt/data  # Set permissions for testing
    ```

    Define the PersistentVolume:
    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: local-pv
    spec:
      capacity:
        storage: 1Gi
      accessModes:
        - ReadWriteOnce
      storageClassName: local-storage
      local:
        path: /mnt/data
      nodeAffinity:
        required:
          nodeSelectorTerms:
          - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
              - node1
    ```

3. **Create the PersistentVolumeClaim (PVC)**:
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: local-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      storageClassName: local-storage
      resources:
        requests:
          storage: 1Gi
    ```

4. **Verify by Deploying a Pod**:
    Now, create a simple Pod that uses the PVC:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: test-pod
    spec:
      containers:
      - name: test-container
        image: busybox
        command: [ "sh", "-c", "echo 'Hello from Kubernetes storage!' > /mnt/test-file && sleep 3600" ]
        volumeMounts:
        - mountPath: /mnt
          name: local-storage-volume
      volumes:
      - name: local-storage-volume
        persistentVolumeClaim:
          claimName: local-pvc
    ```

5. **Test and Verify**:
    - Wait for the Pod to start, then check the contents of the `/mnt/data` directory on the host:
      ```sh
      cat /mnt/data/test-file
      ```

    - You should see the message "Hello from Kubernetes storage!" indicating the PVC is correctly mounting the local storage.

---

### Cleanup:

Once testing is complete, remember to delete the resources:

```sh
kubectl delete -f pod-using-pvc.yaml
kubectl delete -f pvc-local.yaml
kubectl delete -f pv-local.yaml
kubectl delete -f storageclass-local.yaml
```

---

### Explanation:

- **StorageClass**: `kubernetes.io/no-provisioner` is used to indicate that no dynamic provisioner is available, which is suitable for local or host-based storage.
- **Volume Binding Mode**: `WaitForFirstConsumer` ensures the PV is bound to a node where the Pod requesting the PVC will run, providing better affinity and locality for storage.
- **Local PV**: Configures the storage path directly to the filesystem of the node, ideal for testing in a local or virtualized environment.
