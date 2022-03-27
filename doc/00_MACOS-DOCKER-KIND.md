# MacOS: Install Podman & KinD

## Pre Prerequisites
[Common tools](00_MACOS-COMMON-TOOLS.md)

https://www.cprime.com/resources/blog/docker-on-mac-with-homebrew-a-step-by-step-tutorial/

```shell
brew cask install docker
brew install kind@v0.12.0

```

## Kind

https://istio.io/latest/docs/setup/platform-setup/kind/

```shell
# install ...
cat <<EOF | kind create cluster --name istio-playground --wait 5m --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30080
    hostPort: 80
    listenAddress: "0.0.0.0" # Optional, defaults to "0.0.0.0"
    protocol: TCP
  - containerPort: 30443
    hostPort: 443
    listenAddress: "0.0.0.0" # Optional, defaults to "0.0.0.0"
    protocol: TCP
EOF
# ... or just start
docker start istio-payground-control-plane

# check connectivity
kubectl cluster-info --context kind-istio-playground

docker port istio-playground-control-plane
# 6443/tcp -> 127.0.0.1:53526
# 30443/tcp -> 0.0.0.0:443
# 30080/tcp -> 0.0.0.0:80

kind get clusters
kubectl config use-context kind-istio-playground
kubectl cluster-info

```


Install a UI
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
# verify the dashboard is installed
kubectl get pod -n kubernetes-dashboard
# Create a ClusterRoleBinding to provide admin access to the newly created cluster.
kubectl create clusterrolebinding default-admin --clusterrole cluster-admin --serviceaccount=default:default

# To login to Dashboard, you need a Bearer Token. Use the following command to store the token in a variable.
token=$(kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}"|base64 --decode)
echo $token
```

You can enable access to the Dashboard using the kubectl command-line tool, by running the following command:
```shell
kubectl proxy
```

Kubectl will make Dashboard available at 
[http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.)

Uninstall
```shell
kind delete cluster --name istio-playground

```