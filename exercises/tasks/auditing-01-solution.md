# Auditing Task 1: Solution

[< Back to exercise list](../README.md)

[< Back to auditing-01.md](./auditing-01.md)

SSH onto the `master` and become root:

```shell
vagrant ssh master
sudo -i
```

Create the audit config (`/etc/kubernetes/audit.conf`) with the following contents:

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  - level: Metadata
    namespaces: ["prod"]
    verbs: ["get", "delete"]
    resources:
    - group: ""
      resources: ["secrets"]
```

Add the following volumes to the API server config (`/etc/kubernetes/manifests/kube-apiserver.yaml`):

```yaml
  - hostPath:
      path: /etc/kubernetes/audit.conf
      type: File
    name: audit-config
  - hostPath:
      path: /var/log/kube-audit.log
      type: FileOrCreate
    name: audit-log
```

Add the volume mounts:

```yaml
    - mountPath: /etc/kubernetes/audit.conf
      name: audit-config
      readOnly: true
    - mountPath: /var/log/kube-audit.log
      name: audit-log
      readOnly: false
```

Add the following to the the API server arguments:

```yaml
    - --audit-policy-file=/etc/kubernetes/audit.conf
    - --audit-log-path=/var/log/kube-audit.log
    - --audit-log-maxage=90
```

## Validate

On the `master` host, tail the audit log file:

```shell
tail -f /var/log/kube-audit.log
```

Read the `db-pass` secret in the `prod` namespace and check the log file:

```shell
kubectl describe secret db-pass -n prod
```

Delete the `db-pass` secret and check the log file:

```shell
kubectl delete secret db-pass -n prod
```
