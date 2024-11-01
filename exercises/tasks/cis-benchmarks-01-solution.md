# CIS Benchmarks Task 1: Solution

[< Back to exercise list](../README.md)

[< Back to cis-benchmarks-01.md](./cis-benchmarks-01.md)

Based on the remediations section of the report for test `4.1.1` we can see:

```
4.1.1 Run the below command (based on the file location on your system) on the
each worker node. For example, chmod 600 /lib/systemd/system/kubelet.service
```

SSH into `node1` and become root:

```shell
vagrant ssh node1
sudo -i
```

Inspect the current permissions of the kubelet service file:

```shell
ls -la /lib/systemd/system/kubelet.service

-rw-r--r-- 1 root root 278 May 14 11:38 /lib/systemd/system/kubelet.service
```

Set the file permissions as suggested:

```shell
chmod 600 /lib/systemd/system/kubelet.service
```

Verify the change:

```shell
ls -la /lib/systemd/system/kubelet.service

-rw------- 1 root root 278 May 14 11:38 /lib/systemd/system/kubelet.service
```
