
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

User: admin
Pass: prom-operator
```

## Tracing utilizado X-Ray

```bash
Install eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl

Install x-ray and app
eksctl utils associate-iam-oidc-provider --cluster k8s-igti-pos --approve --region sa-east-1
eksctl create iamserviceaccount --name xray-daemon --namespace default --cluster k8s-igti-pos --region sa-east-1 --attach-policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess --approve --override-existing-serviceaccounts
cd x-ray && kubectl create -f xray-k8s-daemonset.yaml
kubectl describe daemonset xray-daemon
kubectl logs -l app=xray-daemon
cd x-ray && kubectl apply -f x-ray-sample-front-k8s.yml
cd x-ray && kubectl apply -f x-ray-sample-back-k8s.yml
kubectl describe deployments x-ray-sample-front-k8s x-ray-sample-back-k8s
kubectl describe services x-ray-sample-front-k8s x-ray-sample-back-k8s
kubectl get service x-ray-sample-front-k8s -o wide

Delete
kubectl delete deployments x-ray-sample-front-k8s x-ray-sample-back-k8s
kubectl delete services x-ray-sample-front-k8s x-ray-sample-back-k8s
cd x-ray && kubectl delete -f xray-k8s-daemonset.yaml
eksctl delete iamserviceaccount --name xray-daemon --cluster k8s-igti-pos --region sa-east-1
```



## Author

👤 **Leandro de Matos Pereira**
