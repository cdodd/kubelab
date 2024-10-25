
# Auditing Task 1

Enable auditing on the cluster with the following requirements:

* Audited resources: secrets
* Actions: read, delete
* Namespace: `prod`
* Log level: metadata
* Audit log file: `/var/log/kube-audit.log`
* Audit config location: `/etc/kubernetes/audit.conf`
* Max log age: 90 days

## Validate

On the `master` host, tail the audit log file:

```shell
tail -f /var/log/kube-audit.log
```

Create the prod namespace and a secret:

```shell
kubectl create namespace prod
kubectl create secret generic db-creds --from-literal user=dbapp -n prod
```

Read the secret and check the log file:

```shell
kubectl describe secret db-creds -n prod
```

Delete the secret and check the log file:

```shell
kubectl delete secret db-creds -n prod
```

## Solution

See [auditing-01-solution.md](./auditing-01-solution.md).
