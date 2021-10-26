# MacOS: Install Podman & KinD

## Pre Prerequisites
[Common tools](MACOS-COMMON-TOOLS.md)

https://www.cprime.com/resources/blog/docker-on-mac-with-homebrew-a-step-by-step-tutorial/

```shell
brew cask install docker
brew install kind@0.11.1

```

## Kind
```shell
# install ...
kind create cluster --name istio-playground
# ... or just start
docker start istio-payground-control-plane

# check connectivity
kubectl cluster-info --context kind-istio-playground

docker port istio-playground-control-plane
# 6443/tcp -> 127.0.0.1:49507

kind get clusters
kubectl config use-context kind-istio-playground
kubectl cluster-info

```