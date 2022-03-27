# Istio playground

## Prerequisites

### 1.- kubectl & helm

In case you don't have these tools:

[Install kubectl and Helm](doc/00_MACOS-COMMON-TOOLS.md)

### 2.- A Kubernetes cluster

In cse you don't have acces to a cluster to test, you have these options to install it locally:
* KinD (Docker): [MacOS: Install Docker Kind](doc/00_MACOS-DOCKER-KIND.md)
* KinD (Podman): [MacOS: Install Podman Kind](doc/00_MACOS-PODMAN-KIND.md)
* Minikube: [MacOS: Install Docker and Minikube](doc/00_MACOS-DOCKER-MINIKUBE.md)

### 3.- Istio

To install Istio on an existing cluster:
* [Install Istio with Helm](doc/01_INSTALL_ISTIO_HELM.md)
* [Install Istio with Istioctl](doc/01_INSTALL_ISTIO_ISTIOCTL.md)

### 4.- Install addons

Next step is to install the following addons:
* Prometheus
* Grafana
* Jaeger
* Kiali

You can follow the instructions [here](doc/02_INSTALL_ADDONS.md)

## Next steps

Once Istio is installed you can install the [Sample application](doc/03_SAMPLE_APPLICATION.md) that demonstrates the 
usage of Istio.