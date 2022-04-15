
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
aws eks --region us-east-1 update-kubeconfig --name nome-do-cluster
aws eks --region us-east-1 update-kubeconfig --name k8s-igti-pos
```

```bash
kubectl get nodes
```

## Deploy o Ingress

```bash
kubectl apply -f kubernetes/traefik/ingress.yml
```

## Deploy demo services

```bash
kubectl apply -f kubernetes/apps/whois.yml
kubectl apply -f kubernetes/apps/faker.yml
kubectl apply -f kubernetes/apps/pudim.yml
```

## Deploy do Metric Server

```bash
kubectl apply -f kubernetes/metric-server/metric-server.yml
```

## Deploy do Prometheus-Operator via Helm

```bash
kubectl apply -f kubernetes/metric-server/metric-server.yml
```


## Author

👤 **Leandro de Matos Pereira**
