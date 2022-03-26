# Install Istio


## Prerequisites

* [Helm](https://helm.sh/docs/intro/install/#through-package-managers)


## Download Istio

See detailed instructions [here](https://istio.io/latest/docs/setup/install/helm/)


```shell
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update

kubectl create namespace istio-system
kubectl create namespace istio-ingress
kubectl label namespace istio-ingress istio-injection=enabled

helm install istio-base istio/base --version 1.13.2 -n istio-system
helm install istiod istio/istiod --version 1.13.2 -n istio-system --wait
#helm install istio-ingress istio/gateway -n istio-ingress --wait
# https://github.com/istio/istio/blob/master/manifests/charts/gateway/templates/service.yaml#L41
# https://github.com/istio/istio/blob/master/manifests/charts/gateway/values.yaml#L43
helm install istio-ingress istio/gateway --version 1.13.2 -n istio-ingress --wait --set service.type=NodePort
  
helm --debug install istio-ingress istio/gateway --version 1.13.2 -n istio-ingress --wait --set service.type=NodePort
  

helm show values istio/base


helm status istio-base -n istio-system
helm status istio-ingress -n istio-ingress

helm get all istio-base -n istio-system
helm get all istio-ingress -n istio-ingress
```

Verify
```shell
kubectl get pods -n istio-system
kubectl get pods -n istio-ingress 
kubectl get services -n istio-ingress istio-ingress -o json | jq -r '.spec.ports'
```

Expose istio-ingress

```shell

# patch the service
# KinD only:
# path istio-ingressgateway to use the ports we've mapped as extraPortMappings in the cluster creation
kubectl -n istio-system patch svc istio-ingress --patch \
  '{"spec": { "ports": [ { "port": 80, "nodePort": 30080 }, { "port": 443, "nodePort": 30443 } ] } }'
  
```

Verify
```shell
# deploy and expose sample service
kubectl create namespace istio-test
kubectl label namespace istio-test istio-injection=enabled
kubectl config set-context --current --namespace=istio-test
kubectl apply -f istio-test.yaml

curl -vv "http://localhost/echo-service"
# HTTP/1.1 200 OK

# clean up resources
kubectl delete -f istio-test.yaml
kubectl delete namespace istio-test
```
