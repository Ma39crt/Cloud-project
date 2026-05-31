# cloud native app ( To-Do app)
a three layer to do application with a front end that is react and a node.js backend with a postrgresql database which was deployed with kuberenets minikube , RBAC and Prometheus/Grafana data analytics

----------------------------------------
# structure

├── frontend/ # React app (multi‑stage Dockerfile)
├── backend/ # Node.js API (Express + PostgreSQL)
├── k8s/ # Kubernetes manifests (deployments, services, network policies, RBAC)
└── screenshots/ # Evidence for logbook

----------------------------------------
# to run locally with docker compose

"docker compose up--build"
then http://localhost:3000

# to start minikube with calico for policy 
minikube start --driver=docker --cpus=4 --memory=7700 --kubernetes-version=v1.28.0 --cni=calico

# to build images in the minikube 
eval $(minikube docker-env)
docker build -t frontend:latest ./frontend
docker build -t backend:latest ./backend

# build
kubectl create namespace todo-app
kubectl apply -f k8s/ -n todo-app

# to runthe app (two powershells)
kubectl port-forward -n todo-app svc/backend-service 5000:5000
kubectl port-forward -n todo-app svc/frontend-service 3000:80

then open http://localhost:3000

----------------------------------------
# security 

network policies
- front end to backend allowed port 5000
- back end to database allowed port 5432
- front end to database blocked when tested with wget 

RBAC
- service account app-viewer-sa with a role to name pods
- test pods (test-pod ) to verify allowance or permisson

----------------------------------------

# monitoring Prometheus and Grafana installed with helm

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring --create-namespace

and to use Grafana 
minikube service -n monitoring monitoring-grafana --url

and kubectl top pods -n todo-app shows how much resource is being used

----------------------------------------

# Serverless & Edge Computing

- openFaas deployed with nodeinfo function and invoked with curl

- k3s edge simulation ran in k3s in docker container pushed nginx service

----------------------------------------
Muneeb Ahmad 