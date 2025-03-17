Helm chart for Kubernetes services selectors exporter
====
Exports Kubernetes services selectors as metrics.

[Source Code](https://github.com/bwsolucoes/kube-service-selectors) | [Docker Image](https://hub.docker.com/r/cloudcose/kube-service-selectors)

```bash
helm install kube-service-selectors cloudcose/kube-service-selectors
```

#### Configuration
Available options are in [values.yaml](values.yaml). Alternatively run
```bash
helm show values cloudcose/kube-service-selectors
```
