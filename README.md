# Kubernetes Core

Deploy a basic Kubernetes bundle - and only consume the resources needed to
run your containerized workloads in Google's container orchestrator
Kubernetes.

## Usage

    juju deploy cs:~containers/bundle/kubernetes-core

### Scaling

    juju add-unit kubernetes

## Credentials

You can download the `kubectl` binary and config from the kubernetes master
by `juju scp kubernetes\[master-unit-number]:kubectl_package.tar.gz .`

Uncompress that file and use the kubectl and config to interact with the
cluster.

# TODO

- Get Credentials
- Reverse Proxy
