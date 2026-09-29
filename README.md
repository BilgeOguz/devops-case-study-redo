# Hello DevOps
This project is to have hands on practice with devops tools and IaaS implementations. 

#### What's required for the project:
- Docker
- minikube/kind
- helm

## Documentation

### Building Docker Image

```bash
docker build -t flask-api:1.0 /app/mvc-flask-pymongo/
```
### Start Docker Compose

```bash
docker compose up
```
### Deployment of the App to the Cluster 

```bash
minikube start
kubectl apply -f k8s/
kubectl config set-context pymongo --namespace=pymongo
kubectl config use-context pymongo
```
+ Get mongodb from helm (no raw k8s files)

```bash
helm install mongodb
```

### Helm

```bash
helm install flask-app 
```
