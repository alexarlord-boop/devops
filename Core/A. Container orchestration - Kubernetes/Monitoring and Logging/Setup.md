# Prometheus setup

|Option|Complexity|
|-|-|
|Creating all configs ```manually```, execute in right order |🤨|
|Using ```operator``` - manager of Prometheus components|😉|
|Using ```Helm chart``` for operator deployment|🤩|


### Using Helm chart option:
[Helm Chart for Prometheus](https://artifacthub.io/packages/helm/prometheus-community/prometheus)

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update

helm install prometheus prometheus-community/kube-prometheus-stack

```

<!-- TODO: important config files -->
### Worth checking out:
1. statefulset for prometheus
```bash
kubectl describe statefulsets prometheus-prometheus-kube-prometheus-prometheus > prom.yaml
```

2. statefulset for alertmanager
```bash
kubectl describe statefulsets alertmanager-prometheus-kube-prometheus-alertmanager > alert.yaml
```

3. deployment for prometheus operator
```bash
kubectl describe deployment prometheus-kube-prometheus-operator > oper.yaml
```


### Accessing Prometheus and Grafana UIs:
```bash
# grafana UI
kubectl port-forward deployments/prometheus-grafana 3000

# prometheus UI
kubectl port-forward pod/prometheus-prometheus-kube-prometheus-prometheus-0 9090:9090 
```
<!-- TODO: adding metrics endpoint -->