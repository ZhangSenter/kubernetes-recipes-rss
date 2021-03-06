# kubernetes-recipes-rss

Configuration files for deploying [RSS Recipes](https://github.com/hora-prediction/recipes-rss-kube) on Kubernetes.

# System requirements
* A cluster of at least 2 nodes (1 master + 2 minions) running kubernetes. The instruction of how to setup kubernetes can be found [here](http://kubernetes.io/v1.0/docs/getting-started-guides/README.html).
* [DNS in kubernetes](https://github.com/kubernetes/kubernetes/blob/v1.0.3/cluster/addons/dns/README.md)

# How to deploy RSS Recipes

RSS Recipes can be deployed on a running kubernetes cluster by executing the following commands on the master node:
* ```kubectl create -f cassandra-service.yaml```
* ```kubectl create -f cassandra-controller.yaml```
* ```kubectl create -f middletier-service.yaml```
* ```kubectl create -f middletier-controller.yaml```
* ```kubectl create -f edge-service.yaml```
* ```kubectl create -f edge-controller.yaml```

These commands will pull and run the corresponding docker images from [Docker Hub](https://hub.docker.com/u/hora/).

The cassandra must be initialized before it can serve the first request. This can be done by executing:

```cqlsh -f create.cql <IP of one of the minions> <NodePort of cassandra service>```

on any machine that can connect to the cluster.

# Horizontal scaling

```kubectl scale rc <Replication controller> --replicas=<number of replicas>```

Example:

```kubectl scale rc edge --replicas=3```

will create 3 replicas for the edge.

# Rolling update

```kubectl rolling-update edge --image=<new image>```

Example:

```kubectl rolling-update edge --image=hora/recipes-rss-kube-edge:0.3```

will update edge containers to version 0.3.

# Deployment screencast

[![asciicast](https://asciinema.org/a/055suasfhroh9pknvg97ql6rt.png)](https://asciinema.org/a/055suasfhroh9pknvg97ql6rt)
