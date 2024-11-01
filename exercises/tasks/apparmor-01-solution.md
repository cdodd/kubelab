# AppArmor Task 1: Solution

[< Back to exercise list](../README.md)

[< Back to apparmor-01.md](./apparmor-01.md)

SSH into `node1`:

```shell
vagrant ssh node1
```

Check AppArmor is loaded and active:

```shell
apparmor_status

apparmor module is loaded.
57 profiles are loaded.
40 profiles are in enforce mode.
[...]
```

Check the name of the new AppArmor profile:

```shell
grep profile /etc/apparmor.d/nginx-profile

profile custom-nginx flags=(attach_disconnected,mediate_deleted) {
```

From this we can see the AppArmor profile name is `custom-nginx`. Note that this
can be different to the filename of the profile.

Next, load the profile:

```shell
apparmor_parser /etc/apparmor.d/nginx-profile
```

Verify the profile is loaded:

```shell
apparmor_status | grep custom-nginx

   custom-nginx
   [...]
```

Back on your main host, create a YAML scaffold (`aa-nginx.yaml`) as a starting
point for the new pod:

```shell
kubectl run aa-nginx --image=nginx --dry-run=client -o yaml > aa-nginx.yaml
```

Modify it to look like the following:

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: aa-nginx
  name: aa-nginx
spec:
  nodeName: node1                     # Additional
  securityContext:                    # Additional
    appArmorProfile:                  # Additional
      type: Localhost                 # Additional
      localhostProfile: custom-nginx  # Additional
  containers:
  - image: nginx
    name: aa-nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

Create the pod:

```shell
kubectl apply -f aa-nginx.yaml
```

## Validate

Check the pod is running:

```shell
kubectl get pod aa-nginx -o json | jq -r .status.phase

Running
```

Check the AppArmor profile is loaded in the pod:

```shell
kubectl get pod aa-nginx -o json | jq .spec.securityContext

{
  "appArmorProfile": {
    "localhostProfile": "custom-nginx",
    "type": "Localhost"
  }
}
```
