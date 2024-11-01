# Auditing Task 1

[< Back to exercise list](../README.md)

Enable auditing on the cluster with the following requirements:

* Audited resources: secrets
* Actions: read, delete
* Namespace: `prod`
* Log level: metadata
* Max log age: 90 days
* Audit log file: `/var/log/kube-audit.log`
* Audit config location: `/etc/kubernetes/audit.conf`

A secret called `db-pass` has been created in the `prod` namespace that can be
used for testing (or create your own).

## Documentation

* https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/

## Solution

See [auditing-01-solution.md](./auditing-01-solution.md).
