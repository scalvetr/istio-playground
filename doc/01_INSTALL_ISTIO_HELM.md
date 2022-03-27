# Install Istio

See detailed instructions [here](https://istio.io/latest/docs/setup/install/helm/)

## Prerequisites

* [Helm](https://helm.sh/docs/intro/install/#through-package-managers)

## Register the Helm repo
```shell
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
```

## Install base packages

```shell
# Create the istio-system namespace
kubectl create namespace istio-system

# deploy the packages to istio-system
helm install istio-base istio/base --version 1.13.2 -n istio-system
helm install istiod istio/istiod --version 1.13.2 -n istio-system --wait
```
Verify

```shell
helm status istio-base -n istio-system
helm get all istio-base -n istio-system

kubectl get pods -n istio-system
```

## Install istio ingress

We'll deploy the ingress gateway controller in a new namespace. The 
[Istio/Gateway](https://istio.io/latest/docs/reference/config/networking/gateway/) configuration must be deployed in 
this same namespace.

```shell
# Create the istio-ingress namespace
kubectl create namespace istio-ingress
kubectl label namespace istio-ingress istio-injection=enabled

# Deploy the ingress gateway in istio-ingress
helm install istio-ingress istio/gateway --version 1.13.2 -n istio-ingress --wait --set service.type=NodePort
```

Notice that the `service.type` value has been changed find here the list of values you can change 
[values.yaml](https://github.com/istio/istio/blob/master/manifests/charts/gateway/values.yaml#L43) and 
[here service.yaml](https://github.com/istio/istio/blob/master/manifests/charts/gateway/templates/service.yaml#L41) where
this value is applied.

To verify the istio ingress we'll run the following commands.

```shell
helm status istio-ingress -n istio-ingress
helm get all istio-ingress -n istio-ingress
kubectl get pods -n istio-ingress

kubectl get services -n istio-ingress istio-ingress -o json | jq -r '.spec.ports'
```

## Expose the istio ingress

In kind we've defined a list of extraPortMappings during the cluster creation. See the [kind cluster creation documentation](00_MACOS-DOCKER-KIND.md)
```shell
# patch the service
kubectl -n istio-ingress patch svc istio-ingress --patch \
  '{"spec": { "ports": [ { "port": 80, "nodePort": 30080 }, { "port": 443, "nodePort": 30443 } ] } }'
  
```

Verify
```shell
# deploy and expose sample service
kubectl create namespace demo-app
kubectl label namespace demo-app istio-injection=enabled

kubectl apply -f https://raw.githubusercontent.com/scalvetr/istio-playground/main/istio-test.yaml

curl -vv "http://localhost/demo-app"
# HTTP/1.1 200 OK

kubectl delete -f https://raw.githubusercontent.com/scalvetr/istio-playground/main/istio-test.yaml
kubectl delete namespace demo-app
```
