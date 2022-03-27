# Install Istio

## Download Istio
See: https://istio.io/latest/docs/setup/getting-started/#download

Download

```shell
rm -fR workdir/

mkdir workdir
cd workdir
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.13.2 sh -
cd istio-1.13.2
export PATH=$PWD/bin:$PATH
```

## Install Istio

See: https://istio.io/latest/docs/setup/install/istioctl/
```shell
istioctl install --set profile=minimal -y

kubectl create namespace istio-ingress
kubectl label namespace istio-ingress istio-injection=enabled

cat <<EOF | kubectl apply -f -
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  name: ingress
spec:
  profile: empty # Do not install CRDs or the control plane
  components:
    ingressGateways:
    - name: ingressgateway
      namespace: istio-ingress
      enabled: true
      label:
        # Set a unique label for the gateway. This is required to ensure Gateways
        # can select this workload
        istio: ingressgateway
  values:
    gateways:
      istio-ingressgateway:
        # Enable gateway injection
        injectionTemplate: gateway
EOF

# KinD only:
# patch svc istio-ingress to use the ports we've mapped as extraPortMappings in the cluster creation
kubectl -n istio-ingress patch svc istio-ingress --patch \
  '{"spec": { "ports": [ { "port": 80, "nodePort": 30080 }, { "port": 443, "nodePort": 30443 } ] } }'

kubectl -n istio-system get svc istio-ingressgateway -o yaml
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
```
