
## Instalação

```sh
terraform init
```

## Aplicando

```sh
terraform apply --auto-approve
```

## Validação

```sh
terraform validate
```

## Adicionando o contexto do nosso cluster ao kubectl

```bash
aws eks --region sa-east-1 update-kubeconfig --name nome-do-cluster
aws eks --region sa-east-1 update-kubeconfig --name k8s-igti-pos
```

```bash
kubectl get nodes
```

## Deploy o Ingress

```bash
kubectl apply -f kubernetes/traefik/ingress.yml
```

## Deploy do Metric Server

```bash
kubectl apply -f kubernetes/metric-server/metric-server.yml
```

## Deploy do Prometheus-Operator via Helm para monitoração do Cluster

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus-stack prometheus-community/kube-prometheus-stack --create-namespace monitoring
kubectl port-forward service/prometheus-stack-grafana 3000:80 -n monitoring
```

## Deploy demo services

```bash
kubectl apply -f kubernetes/apps/app-produtos.yaml
kubectl apply -f kubernetes/apps/app-loja.yaml
kubectl apply -f kubernetes/apps/app-ingress.yaml
```

## Author

👤 **Leandro de Matos Pereira**
