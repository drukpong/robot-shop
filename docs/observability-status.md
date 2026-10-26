# Observability Status

## Deployment
Robot Shop is deployed on Amazon EKS in us-east-1.
The Helm release is robot-shop, in namespace robot-shop.
The external storefront returned HTTP 200 OK during verification.

## Monitoring
A Prometheus scrape configuration is maintained in
observability/prometheus/prometheus.yml.

At the time of verification, no Prometheus or Grafana release was
installed in the cluster. The Kubernetes Metrics API was unavailable,
and `kubectl top nodes` failed because the metrics API service was absent.

## Capacity and risk
The cluster has three t3.small worker nodes. Reported memory requests
exceeded allocatable memory on two nodes. Installing a full monitoring
stack without increasing capacity could disrupt application workloads.

## Remaining work
- Deploy Prometheus and Grafana when adequate capacity is available.
- Provision a dashboard as code and verify its panels with live data.
- Configure centralized logging with Fluent Bit and Loki.
- Instrument a service with OpenTelemetry and verify traces in Jaeger.
- Configure and test SLO alerts.
