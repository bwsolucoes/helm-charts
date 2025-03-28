CloudCoSe Kubernetes Cost Metrics Collector
====
Ready-to-deploy setup of components necessary for Kubernetes cost management and FinOps in CloudCoSe.

Components
- [Prometheus](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus)
  - [kube-state-metrics](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics)
  - [prometheus-node-exporter](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-node-exporter)
  - [prometheus-pushgateway](https://github.com/walker-tom/helm-charts/tree/main/charts/prometheus-pushgateway)
- [kube-service-selectors](https://github.com/bwsolucoes/helm-charts/tree/main/charts/kube-service-selectors)

## Prerequisites
Prerequisites are based on dependencies:
- Kubernetes 1.17+
- Helm 3+

## Description
Helm chart for the CloudCoSe Kubernetes Collector project, which is created to collect Kubernetes resources information and share it with CloudCoSe FinOps project - https://cloudcose.com/cloudcose/.

## Installation
One release per cluster
```bash
helm install kube-cost-metrics-collector cloudcose/kube-cost-metrics-collector \
--set prometheus.server.dataSourceId=<data-source-id> \
--set prometheus.server.username=<username> \
--set prometheus.server.password=<password> \
--namespace cloudcose \
--create-namespace
```

*Override **cloudcose** remote write target to send metrics to [open source CloudCoSe](https://github.com/bwsolucoes/cloudcose)*
```bash
helm install kube-cost-metrics-collector cloudcose/kube-cost-metrics-collector \
--set prometheus.server.dataSourceId=<data-source-id> \
--set prometheus.server.username=<username> \
--set prometheus.server.password=<password> \
--set prometheus.server.remote_write[0].url=https://<ip-address>/storage/api/v2/write \
--set prometheus.server.remote_write[0].name=cloudcose \
--namespace cloudcose \
--create-namespace
```

## Upgrade
```bash
helm upgrade kube-cost-metrics-collector cloudcose/kube-cost-metrics-collector \
--namespace cloudcose
```

### Version 0.1.1
Prometheus chart was upgraded from 15.10.0 to 17.0.2.
Because of changes in [Prometheus](https://github.com/prometheus-community/helm-charts/tree/prometheus-17.0.2/charts/prometheus#upgrading-chart) chart please do the following before upgrade:
```bash
kubectl delete --namespace cloudcose daemonset kube-cost-metrics-collector-prometheus-node-exporter
kubectl delete --namespace cloudcose deployments kube-cost-metrics-collector-prometheus-pushgateway
kubectl scale deployments --namespace cloudcose kube-cost-metrics-collector-prometheus-server --replicas=0
```

## Configuration
The following table lists the commonly used configurable parameters of the Helm chart.

Parameter | Description
--------- | ------------------------------------------------
prometheus.server.dataSourceId | CloudCoSe Kubernetes data source id
prometheus.server.username | username which is used on CloudCoSe Kubernetes data source registration
prometheus.server.password | password which is used on CloudCoSe Kubernetes data source registration
prometheus.kubeStateMetrics.enabled | set to false if external kube-state-metrics exists and accessible
prometheus.prometheus-node-exporter.enabled | set to false if external node-exporter exists and accessible
prometheus.prometheus-pushgateway.enabled | set to false if external pushgateway exists and accessible

Additional options are in [values.yaml](values.yaml). Alternatively run
```bash
helm show values cloudcose/kube-cost-metrics-collector
```
