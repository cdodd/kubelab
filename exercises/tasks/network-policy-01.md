# Network Policy Task 1

[< Back to exercise list](../README.md)

Pod `app1` in namespace `dev` makes HTTP requests to pod `web` in namespace
`prod` via a service called `web`. You can view the logs of `app1` to see the
successful requests.

Create a default deny ingress network policy for all pods in the `prod`
namespace, then create a network policy to allow requests specifically from
`app1` in the `dev` namespace to the `web` pod in the `prod` namespace.

## Documentation

* https://kubernetes.io/docs/concepts/services-networking/network-policies/

## Solution

See [network-policy-01-solution.md](./network-policy-01-solution.md).
