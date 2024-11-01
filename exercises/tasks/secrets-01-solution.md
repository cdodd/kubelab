# Secrets Task 1: Solution

[< Back to exercise list](../README.md)

[< Back to secrets-01.md](./secrets-01.md)

## Inspect and Read the Secret

Inspect the pod to get information about the secret in use:

```shell
kubectl get pod web -n prod -o json | jq .spec.containers

[
  {
    "env": [
      {
        "name": "DB_PASSWORD",
        "valueFrom": {
          "secretKeyRef": {
            "key": "DB_PASSWORD",
            "name": "db-pass"
          }
        }
      }
    ],
    "image": "nginx",
    "imagePullPolicy": "Always",
    "name": "nginx",
    "resources": {},
    "terminationMessagePath": "/dev/termination-log",
    "terminationMessagePolicy": "File",
    "volumeMounts": [
      {
        "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
        "name": "kube-api-access-m84pv",
        "readOnly": true
      }
    ]
  }
]
```

From this we can see that `DB_PASSWORD` from the `db-pass` secret is set as an
environment variable.

Inspect the secret resource:

```shell
kubectl get secret db-pass -n prod -o json

{
    "apiVersion": "v1",
    "data": {
        "DB_PASSWORD": "SUREUUQ="iff
    },
    "kind": "Secret",
    "metadata": {
        "creationTimestamp": "2024-11-01T20:13:53Z",
        "name": "db-pass",
        "namespace": "prod",
        "resourceVersion": "5005",
        "uid": "c8ce18e3-2fd1-4eaa-a956-b3c703ca4abe"
    },
    "type": "Opaque"
}
```

Decode the `DB_PASSWORD` value:

```shell
kubectl get secret db-pass -n prod -o json | jq -r .data.DB_PASSWORD | base64 -d

IDDQD
```

Or, simply read the secret from the environment variables of the running pod:

```shell
kubectl exec web -n prod -- env | grep DB_PASSWORD

DB_PASSWORD=IDDQD
```

## Recreate the Pod

Save the config of the existing `web` pod:

```shell
kubectl get pod web -n prod -o yaml > web-pod.yaml
```

Remove the environment variable section (watch out for the YAML syntax):

```yaml
  - env:
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          key: DB_PASSWORD
          name: db-pass
```

Add a volume to the `spec` section:

```yaml
  volumes:
  - name: secret-volume
    secret:
      secretName: db-pass
```

Add a volume mount to the container:

```yaml
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: "/mnt/secret"
```

Delete and recreate the pod:

```shell
kubectl delete pod web -n prod
kubectl apply -f web-pod.yaml
```

Check the secret can be read as expected:

```shell
kubectl exec web -n prod -- cat /mnt/secret/DB_PASSWORD

IDDQD
```
