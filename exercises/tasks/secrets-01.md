# Secrets Task 1

[< Back to exercise list](../README.md)

A pod `web` has been created the in `prod` namespace. It makes use of a secret
as an environment variable.

1. Inspect the secret and read the base64 encoded value
2. Recreate this pod, mounting the secret as a read-only volume at `/mnt/secret`
   instead of setting an environment variable.

## Documentation

* https://kubernetes.io/docs/concepts/configuration/secret/

## Solution

See [secrets-01-solution.md](./secrets-01-solution.md).
