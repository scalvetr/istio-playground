# Install Istio

## Prerequisites
You need a kubernetes cluster

[MacOS: Install Docker Kind](00_MACOS-DOCKER-KIND.md)
[MacOS: Install Podman Kind](00_MACOS-PODMAN-KIND.md)
[MacOS: Install Docker and Minikube](00_MACOS-DOCKER-MINIKUBE.md)

## Download Istio
See: https://istio.io/latest/docs/setup/getting-started/#download

Download

```shell
rm -fR workdir/

mkdir workdir
cd workdir
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.12.1 sh -
cd istio-1.12.1
export PATH=$PWD/bin:$PATH
```

## Install Istio


See: https://istio.io/latest/docs/setup/install/istioctl/
```shell
istioctl install --set profile=demo -y

# KinD only:
# path istio-ingressgateway to use the ports we've mapped as extraPortMappings in the cluster creation
kubectl -n istio-system patch svc istio-ingressgateway --patch \
  '{"spec": { "ports": [ { "port": 80, "nodePort": 30080 }, { "port": 443, "nodePort": 30443 } ] } }'

kubectl -n istio-system get svc istio-ingressgateway -o yaml
```

## Install Kiali

```shell
kubectl apply -f samples/addons
kubectl rollout status deployment/kiali -n istio-system

# see the dashboard
istioctl dashboard kiali
```
