# OpenTelemetry Demo Helm Chart

The helm chart installs [OpenTelemetry Demo](https://github.com/open-telemetry/opentelemetry-demo)
in kubernetes cluster.

## Prerequisites

- Kubernetes 1.23+
- Helm 3.9+

Since the OpenTelemetry demo does not build images targeting arm64 architecture **the chart is not supported in clusters running on
arm64 architectures**, such as kind/minikube running on Apple Silicon.

## Installing the Chart

Download this repo via git CLI

```console
git clone https://github.com/dambott/opentelemetry-helm-chartsv3
```

Or via zip 

```console
curl -LO https://github.com/dambott/opentelemetry-helm-chartsv3/archive/refs/heads/main.zip
unzip main.zip
```

Configure the *values.yaml* to change authentication (WF Token or CSP) and Clustername Line 51

```console
cd opentelemetry-helm-chartsv3*
cd charts/opentelemetry-demo/
vim values.yaml
```

To install the chart with the release name my-otel-demo, run the following command:

```console
kubectl create ns my-otel-demo
helm install my-otel-demo -n my-otel-demo .
```

## Access demo

Use proxy forward to access demo

```console
kubectl port-forward -n my-otel-demo service/my-otel-demo-frontendproxy 8080:8080
```

Or apply ingress for longterm

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-otel-demo
  namespace: my-otel-demo
spec:
  rules:
  - host: "myhostname"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: my-otel-demo-frontendproxy
            port:
              number: 8080
```

