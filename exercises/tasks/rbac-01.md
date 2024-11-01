  # RBAC Task 1

[< Back to exercise list](../README.md)

Create the following resources in the the `dev` namespace:

* A role called `dev-rw` with permissions to:
  * create, delete, read and list pods
  * read and list secrets
* A service account called `dev-app`
* A role binding for the role and service account
* Create a pod in the `dev` namespace that uses the service account

## Documentation

https://kubernetes.io/docs/reference/access-authn-authz/rbac/

## Solution

See [rbac-01-solution.md](./rbac-01-solution.md).
