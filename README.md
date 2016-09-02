# Core Kubernetes Bundle

This is a Juju bundle that creates a basic Kubernetes cluster. This bundle 
deploys the minimum number of resources needed to get started using Google's 
container orchestrator, Kubernetes.

## Overview

Kubernetes is an open-source platform for automating deployment, operations,
and scaling of containerized applications across clusters of hosts.

It groups containers that make up an application into logical units for easy
management and discovery. You can scale up the individual pieces to create a 
larger cluster.

## Usage

To deploy a Kubernetes cluster it is one Juju command:

```
juju deploy kubernetes-core
```

Juju deployed two units of the kubernetes charm, three units of etcd and 
related them to form a Kubernetes cluster. One of the kubenetes units is the
master and the other is a worker. To determine which unit is the master use the
`juju status` command.

### Using kubectl

After the cluster is deployed download the `kubectl` command line tool and
the configuration from the master unit. For your convenience, the master unit
created a package with the certificate, key and configuration to securely
communicate with this Kubernetes cluster. If the master unit changes, say you
deploy a new cluster you will need to download the new package as the 
information has changed.

```
juju scp kubernetes/<leader-unit-number>:kubectl_package.tar.gz .
tar zxf kubectl_package.tar.gz
./kubectl --kubeconfig ./kubeconfig get pods
```

You should now have the `kubectl` command and configuration for the cluster
that was just created, you can now check the state of the cluster:

```
./kubectl --kubeconfig ./kubeconfig cluster-info
```

Create pods and services on the Kubernetes cluster by using resource 
definitions on your host system:

```
./kubectl --kubeconfig ./kubeconfig create -f example.yml

```

List all pods in the cluster:

```
./kubectl --kubeconfig ./kubeconfig get pods
```

List all services in the cluster:

```
./kubectl --kubeconfig ./kubeconfig get svc
```

You should be able to manage the Kubernetes cluster by using the `kubectl` 
command line tool. Use the 
[kubectl-cheatsheet](http://kubernetes.io/docs/user-guide/kubectl-cheatsheet/) 
for more information about the `kubectl` command.

## Scale out Usage

Any of the services provided can be scaled out post-deployment. By default
the pods are automatically be spread throughout the Kubernetes clusters you
have deployed.

### Scaling Kubernetes Up

To add more Kubernetes nodes to the cluster:

    juju add-unit kubernetes

or specify machine constraints to create larger nodes:

    juju add-unit kubernetes --constraints "cpu-cores=8 mem=32G"

Refer to the
[machine constraints documentation](https://jujucharms.com/docs/stable/charms-constraints)
if you want more information about setting larger constraints.

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
