
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
## Deploy demo services

```bash
kubectl apply -f kubernetes/apps/app-produtos.yaml
kubectl apply -f kubernetes/apps/app-loja.yaml
kubectl apply -f kubernetes/apps/app-ingress.yaml
```

## Deploy do Prometheus-Operator via Helm para monitoração do Cluster

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus-stack prometheus-community/kube-prometheus-stack
kubectl port-forward service/prometheus-stack-grafana 3000:80
```

## Tracing utilizado X-Ray

```bash
Install eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl

eksctl utils associate-iam-oidc-provider --cluster k8s-igti-pos --approve --region sa-east-1
eksctl create iamserviceaccount --name xray-daemon --namespace default --cluster k8s-igti-pos --region sa-east-1 --attach-policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess --approve --override-existing-serviceaccounts

```



## Author

👤 **Leandro de Matos Pereira**
