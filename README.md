# Istio playground

## Prerequisites

* Docker or Podman
* Kind
* Kubectl

[Setup MacOS](INSTALL-MACOS.md)

## Setup new kubernetes cluster

```shell
# Connect to the cluster
podman machine start

# install ...
kind create cluster --name istio-playground
# ... or just start
podman start istio-payground-control-plane

# check connectivity
kubectl cluster-info --context kind-istio-playground

podman port istio-playground-control-plane
# 6443/tcp -> 127.0.0.1:49507

kind get clusters
kubectl config use-context kind-istio-playground
kubectl cluster-info


```
