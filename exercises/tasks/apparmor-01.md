# AppArmor Task 1

[< Back to exercise list](../README.md)

An Apparmor profile has been created on `node1` at
`/etc/apparmor.d/nginx-profile`. Create a pod with the following requirements:

* Name: `aa-nginx`
* Namespace: `default`
* Image: `nginx`
* Use the AppArmor profile in `/etc/apparmor.d/nginx-profile`
* Make sure the pod runs on `node1`

## Documentation

* https://kubernetes.io/docs/tutorials/security/apparmor/
* https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/

## Solution

See [apparmor-01-solution.md](./apparmor-01-solution.md).
