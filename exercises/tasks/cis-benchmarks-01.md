# CIS Benchmarks Task 1

[< Back to exercise list](../README.md)

A CIS benchmark report has been created for `node1` using a
[kube-bench](https://github.com/aquasecurity/kube-bench) job. It can be viewed
with this (_somewhat unfriendly_) command:

```shell
kubectl logs $(kubectl get pods --selector=job-name=kube-bench-node1 -o json | jq -r '.items[].metadata.name')

[INFO] 4 Worker Node Security Configuration
[INFO] 4.1 Worker Node Configuration Files

[...]
```

Fix the failure in check `4.1.1` on `node1`.

## Solution

See [cis-benchmarks-01-solution.md](./cis-benchmarks-01-solution.md).
