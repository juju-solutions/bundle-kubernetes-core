# Core Kubernetes Bundle

This is a bundle that creates a basic Kubernetes cluster.

## Overview

Kubernetes is an open-source system for automating deployment, operations, and
scaling of containerized applications.

It groups containers that make up an application into logical units for easy
management and discovery.


Deploy a basic Kubernetes bundle - and only consume the resources needed to
run your containerized workloads in Google's container orchestrator
Kubernetes.

## Usage
```
juju deploy kubernetes-core
```

The charms
update the status messages with progress, so it is recommended to run
`watch juju status` to monitor the cluster coming up. After it is deployed you
need to grab the kubectl and configuration from the Kubernetes leader node to
control the cluster:

```
juju scp kubernetes/<leader-unit-number>:kubectl_package.tar.gz .
tar zxf kubectl_package.tar.gz
./kubectl --kubeconfig ./kubeconfig get pods
```

You should now have the kubectl command and configuration for the cluster that
was just created, you can now check the state of the cluster:

```
./kubectl --kubeconfig ./kubeconfig cluster-health
```
Now you can run pods inside the Kubernetes cluster:
```
./kubectl --kubeconfig ./kubeconfig create -f example.yaml
```
List all pods in the cluster:
```
./kubectl --kubeconfig ./kubeconfig get pods
```
List all services in the cluster:
```
./kubectl --kubeconfig ./kubeconfig get svc
```

## Scale out Usage

Any of the services provided can be scaled out post-deployment. By default
the pods are automatically be spread throughout the Kubernetes clusters you
have deployed.

### Scaling Kubernetes

To add more Kubernetes nodes to the cluster:

    juju add-unit kubernetes

or specify machine constraints to create larger nodes:

    juju add-unit kubernetes --constraints "cpu-cores=8 mem=32G"

Refer to the
[machine constraints documentation](https://jujucharms.com/docs/stable/charms-constraints)
for other machine constraints that might be useful for the Kubernetes nodes.

### Scaling Etcd

Etcd is used as a key-value store for the Kubernetes cluster. For reliability
the cluster defaults to three instances in this bundle.

For more scalability, we recommend between 3 and 9 etcd nodes. If you want to
add more nodes with `juju add-unit etcd`. The CoreOS etcd documentation has a
chart for the [optimal cluster size](https://coreos.com/etcd/docs/latest/admin_guide.html#optimal-cluster-size)
to determine fault tolerance.

## Known Limitations and Issues

 The following are known issues and limitations with the bundle and charm code:

 - This bundle is not supported on LXD because Juju needs to use a LXD profile
that can run Docker containers.
 - Killing the the Kubernetes leader will result in loss of public key
infrastructure (PKI).
 - No easy way to address the pods from the outside world.
 - The storage feature with ZFS does not work with trusty at this time because
the code has to be enhanced to load the zfs module.

# Kubernetes information

- [Kubernetes homepage](http://kubernetes.io/)
- [Kubernetes documentation](http://kubernetes.io/docs/)
- [Kubernetes github](https://github.com/kubernetes/kubernetes/)
- [Getting started with kubernetes and Juju](http://kubernetes.io/docs/getting-started-guides/juju/)
