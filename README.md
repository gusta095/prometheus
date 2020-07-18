# Stack Prometheus

## Executando projeto

```
kubectl create namespace prometheus
kubectl apply -f prometheus
kubectl apply -f grafana
```

## Imagens do projeto

- Prometheus
    - quay.io/prometheus/prometheus:v2.11.1 (Atualizada)
    - quay.io/prometheus/prometheus:v2.19.1

- Alertmaneges
    - quay.io/prometheus/alertmanager:v0.16.0 (Atualizada)
    - quay.io/prometheus/alertmanager:v0.21.0

- node-export
    - quay.io/prometheus/node-exporter:v0.17.0 (Atualizada)
    - quay.io/prometheus/node-exporter:v1.0.1

- kube-metrics
    - quay.io/coreos/kube-state-metrics:v1.5.0 (Atualizada)
    - quay.io/coreos/kube-state-metrics:v1.9.7

- Grafana
    - grafana/grafana:6.0.1 (Atualizada)
    - grafana/grafana:7.0.3-ubuntu
