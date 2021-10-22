# Istio playground

## Prerequisites

* Docker or Podman
* Kind
* Kubectl

[Setup MacOS](INSTALL-MACOS.md)

## Install Istio

```shell
# Connect to the cluster
podman machine start
podman start istio-payground-control-plane

podman port istio-payground-control-plane
# 6443/tcp -> 127.0.0.1:51465

kind get clusters
kubectl config use-context kind-istio-payground
kubectl cluster-info


```
