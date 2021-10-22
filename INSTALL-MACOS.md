# Install environment on Mac OS

## Pre Prerequisites

```shell
xcode-select --install
```
### Podman

[Podman Installation](https://podman.io/getting-started/installation#macos)

```shell
# The Mac client is available through Homebrew:
brew install podman@3.4.0
echo "alias docker=podman" >> ~/.zshrc
echo "autoload -U compinit; compinit" >> ~/.zshrc
echo "source <(kubectl completion zsh)" >> ~/.zshrc

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

### Kubernetes tools

```shell
brew install kubectl@1.22.2
brew install kind@0.11.1

# test
kind create cluster --name istio-payground

kubectl cluster-info --context kind-istio-payground
```