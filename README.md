Kubernetes Multi-Service App with Ingress

This project demonstrates deploying a three-tier application on Kubernetes with:

Frontend: NGINX serving a static HTML page

Backend: httpbin API service

Database: PostgreSQL (StatefulSet + PVC)

Ingress: Path-based and host-based routing using NGINX Ingress Controller


✅ Prerequisites

https://minikube.sigs.k8s.io/docs/start/ installed

kubectl CLI installed

https://stedolan.github.io/jq/ (optional for pretty JSON output)

Basic knowledge of Kubernetes


🚀 Steps to Deploy
1. Start Minikube and Enable Ingress

minikube start

minikube addons enable ingress

kubectl get pods -n ingress-nginx

2. Apply Application Manifests

This includes:

PostgreSQL Secret, StatefulSet, Service

Backend Deployment, Service

Frontend ConfigMap, Deployment, Service

3. Configure Ingress (Path-Based Routing)

Apply

4. Update /etc/hosts:

   <minikube's IP>	myapp.local
   
   <minikube's IP>	frontend.local
   
   <minikube's IP>	api.local
   
5. Open in browser: http://myapp.local

6. Validate Database Connection

Run a temporary psql client pod:

kubectl run psql-client -n demo --rm -it --image=postgres:15-alpine -- bash -lc '

export PGPASSWORD=supersecret

psql -h postgres -U appuser -d appdb -c "CREATE TABLE IF NOT EXISTS demo(id serial primary key, note text);" &&

psql -h postgres -U appuser -d appdb -c "INSERT INTO demo(note) VALUES ('"'"'hello from k8s'"'"');" &&

psql -h postgres -U appuser -d appdb -c "SELECT * FROM demo;"

'

7. Cleanup

kubectl delete -n demo ingress myapp-front myapp-api myapp-hosts

kubectl delete -f demo-app.yaml

kubectl delete ns demo

