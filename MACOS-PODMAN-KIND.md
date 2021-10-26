# MacOS: Install Podman & KinD

## Pre Prerequisites
[Common tools](MACOS-COMMON-TOOLS.md)

```shell
brew install podman@3.4.0
brew install kind@0.11.1
```
## Podman
[Podman Installation](https://podman.io/getting-started/installation#macos)

```shell
# The Mac client is available through Homebrew:
echo "alias docker=podman" >> ~/.zshrc
echo "autoload -U compinit; compinit" >> ~/.zshrc

# You can then verify the installation information using:
podman info

# To start the Podman-managed VM:
podman machine init
podman machine start
# Load ip6_tables to machine 
podman machine ssh
# run these commands inside the instance
# see: https://kind.sigs.k8s.io/docs/user/rootless/#host-requirements
sudo su -
mkdir -p /etc/systemd/system/user@.service.d
cat > /etc/systemd/system/user@.service.d/delegate.conf << EOF
[Service]
Delegate=yes
EOF
systemctl daemon-reload

cat > /etc/modules-load.d/iptables.conf << EOF
ip6_tables
ip6table_nat
ip_tables
iptable_nat
EOF

podman machine stop
podman machine start
# modprobe -v ip6_tables

exit
exit
# 
```

## Kind
```shell
# Connect to the cluster
podman machine start

# install ...
KIND_EXPERIMENTAL_PROVIDER=podman kind create cluster --name istio-playground
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

## Uninstall

```shell
# delete the cluster
kind delete cluster --name istio-playground
podman stop istio-payground-control-plane
podman rm istio-payground-control-plane

# delete the podman machine
podman machine stop
podman machine rm

# delete the kubectl configuration
kubectl config delete-context kind-istio-playground
kubectl config delete-user kind-istio-playground
kubectl config delete-cluster kind-istio-playground

# uninstall the software
brew uninstall podman
brew uninstall kind
```