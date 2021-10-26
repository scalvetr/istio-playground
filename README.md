# Istio playground

## Prerequisites
You need a kubernetes cluster

[MacOS: Install Podman Kind](MACOS-PODMAN-KIND.md)
[MacOS: Install Docker and Minikube](MACOS-DOCKER-MINIKUBE.md)

## Install Istio

https://istio.io/latest/docs/setup/platform-setup/kind/

Install a UI
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml
# verify the dashboard is installed
kubectl get pod -n kubernetes-dashboard
# Create a ClusterRoleBinding to provide admin access to the newly created cluster.
kubectl create clusterrolebinding default-admin --clusterrole cluster-admin --serviceaccount=default:default

# To login to Dashboard, you need a Bearer Token. Use the following command to store the token in a variable.
token=$(kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}"|base64 --decode)

```

[Click to see the UI](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)

Install 

See: https://istio.io/latest/docs/setup/install/helm/

See: https://istio.io/latest/docs/setup/getting-started/#download

Download
```shell
mkdir workdir
cd workdir
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.11.4 sh -
cd istio-1.11.4
```

Install
```shell
kubectl create namespace istio-system
helm install istio-base manifests/charts/base -n istio-system
helm install istiod manifests/charts/istio-control/istio-discovery \
    -n istio-system

# (Optional) Install the Istio ingress gateway chart which contains the ingress gateway components:
helm install istio-ingress manifests/charts/gateways/istio-ingress \
    -n istio-system

# (Optional) Install the Istio egress gateway chart which contains the egress gateway components:
helm install istio-egress manifests/charts/gateways/istio-egress \
    -n istio-system
```
