# Network Policy Task 1: Solution

[< Back to exercise list](../README.md)

[< Back to network-policy-01.md](./network-policy-01.md)

Create a default deny ingress policy for the `prod` namespace:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: prod
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

Check the logs of `app1` in namespace `dev` to see that requests are now failing.

```shell
kubectl logs -f app1 -n dev
```

Apply the config below to allow requests from `app1` to `web`:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app1
  namespace: prod
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: dev
      podSelector:
        matchLabels:
          app: app1
```

Check the logs of `app1` to see that requests are now successful.
