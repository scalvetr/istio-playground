# Install Addons

## Prometheus
```shell
kubectl apply -n ${ISTIO_SYSTEM_NAMESPACE} -f https://raw.githubusercontent.com/istio/istio/1.13.2/samples/addons/prometheus.yaml
```

## Jaeger
```shell
kubectl apply -n ${ISTIO_SYSTEM_NAMESPACE} -f https://raw.githubusercontent.com/istio/istio/1.13.2/samples/addons/jaeger.yaml
```

## Grafana
```shell
kubectl apply -n ${ISTIO_SYSTEM_NAMESPACE} -f https://raw.githubusercontent.com/istio/istio/1.13.2/samples/addons/grafana.yaml
```

## Kiali
```shell
kubectl apply -n ${ISTIO_SYSTEM_NAMESPACE} -f https://raw.githubusercontent.com/istio/istio/1.13.2/samples/addons/kiali.yaml
```

With istioctl
```shell
istioctl dashboard kiali
```

Without istioctl
```shell
kubectl port-forward svc/kiali 20001:20001 -n istio-system
```
