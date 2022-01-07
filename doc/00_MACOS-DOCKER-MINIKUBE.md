# MacOS: Install Podman & KinD

## Pre Prerequisites
[Common tools](00_MACOS-COMMON-TOOLS.md)

*Docker*: https://www.cprime.com/resources/blog/docker-on-mac-with-homebrew-a-step-by-step-tutorial/

*Minikube*: https://minikube.sigs.k8s.io/docs/start/

```shell
brew cask install docker
brew install minikube@1.23.2

```

## Kind
```shell

minikube config set driver docker

minikube start --memory=16384 --cpus=4 --kubernetes-version=v1.20.2 --profile istio-playground
# ... or just start
docker start istio-payground-control-plane

# check connectivity
kubectl cluster-info --context kind-istio-playground

docker port istio-playground-control-plane
# 6443/tcp -> 127.0.0.1:49507_

kind get clusters
kubectl config use-context kind-istio-playground
kubectl cluster-info

```