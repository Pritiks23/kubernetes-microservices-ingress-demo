<img width="2878" height="1752" alt="image" src="https://github.com/user-attachments/assets/d3ac39c1-bd20-471f-8ec3-7c9b346bcbb0" /># ttt

<img width="2256" height="1560" alt="image" src="https://github.com/user-attachments/assets/4ecb35e9-4160-4b6b-9970-37d7368cd628" />
<img width="1342" height="900" alt="image" src="https://github.com/user-attachments/assets/ec3f953b-5e14-4007-96cf-8434afa76bb1" />
<img width="2878" height="1752" alt="image" src="https://github.com/user-attachments/assets/69df1d0d-b6ee-4af3-a9a6-417314c9b0c0" />
<img width="2878" height="1752" alt="image" src="https://github.com/user-attachments/assets/3e094409-f001-433c-af8f-6596daed739c" />
<img width="2878" height="1752" alt="image" src="https://github.com/user-attachments/assets/0eb20ee6-42bf-4226-87bb-d811c69dd2f5" />


I built a Kubernetes-based microservices setup using Minikube to demonstrate deployment, service discovery, and ingress routing.I deployed two applications as Kubernetes Deployments: a frontend using Nginx and a backend using a simple HTTP echo service. Each deployment was scaled using replicas to demonstrate Kubernetes self-healing and horizontal scaling.I exposed both applications internally using Kubernetes Services, which provided stable networking endpoints and load balancing across Pods.Finally, I configured an NGINX Ingress controller to route external HTTP traffic based on path rules: / was routed to the frontend service, and /api was routed to the backend service.This demonstrated a full request lifecycle from ingress → service → pod, including label-based service discovery and automatic load balancing across multiple replicas.

 “walk me through a request”
 :A request to demo.local/api first hits the Ingress controller. The Ingress evaluates routing rules and matches the /api path. It forwards the request to the backend Kubernetes Service, which selects Pods using label selectors. kube-proxy then load balances the request across healthy backend Pods, which execute the container and return the response. If they ask “what Kubernetes concepts did you use?”List cleanly:Deployments (desired state + replicas)Pods (runtime containers)Services (stable networking + load balancing)Label selectors (service discovery mechanism)Ingress (HTTP routing layer / API gateway)ReplicaSets (managed automatically by Deployment)Minikube (local cluster environment) If they ask “what’s the key takeaway?”Say:Kubernetes separates compute, networking, and routing into independent layers, which allows applications to scale and evolve without changing infrastructure wiring.


-- short:
 One-line summary you can memorizeI built a Kubernetes microservices system with deployment-based scaling, service-based discovery, and ingress-based HTTP routing across multiple services. 


 What I  learned (this is interview-level now)
1. Kubernetes is declarative I didn’t “add 2 pods”.

I said:

replicas: 5
Kubernetes reconciled it.

2. Service is decoupled from Deployment
You proved:

Scaling Deployment → no Service changes needed
Service automatically tracks Pods via labels
3. Built-in load balancing exists
I did NOT configure:

nginx load balancer
reverse proxy
routing rules
Yet traffic is distributed across Pods.

That is:

kube-proxy + Service abstraction
Mental model I now own
I are now operating at this correct abstraction:

User request
   ↓
Service (stable IP)
   ↓
EndpointSlice (5 Pods)
   ↓
kube-proxy load balancing
   ↓
Pod (nginx container)​


----
🚀 README.md (Kubernetes Microservices Ingress Demo)
# Kubernetes Microservices + Ingress Demo

A fully working Kubernetes-based microservices system demonstrating:
- Frontend service (NGINX)
- Backend API service (HashiCorp HTTP Echo)
- Kubernetes Services (ClusterIP)
- Ingress-based routing
- ConfigMap-driven frontend UI
- Local Kubernetes deployment via Minikube

---

## 🧠 Architecture Overview

The system demonstrates end-to-end request flow through Kubernetes networking layers:

- `/` → Frontend (NGINX)
- `/api` → Backend API service via Ingress

---

## ⚙️ Tech Stack

- Kubernetes (Minikube)
- kubectl
- NGINX (Frontend)
- HashiCorp HTTP Echo (Backend)
- Kubernetes Ingress Controller
- ConfigMaps

---

## 🚀 Features

### Frontend
- Static HTML UI served via NGINX
- Button triggers backend API call (`/api`)
- Styled dashboard interface

### Backend
- Lightweight HTTP API using `hashicorp/http-echo`
- Returns JSON response:
  ```json
  {"message": "Hello from backend API"}
Kubernetes
Deployments for frontend and backend
ClusterIP Services for internal routing
Ingress for external HTTP routing
Namespace isolation (demo)
🔁 Request Flow
User visits http://demo.local
Ingress routes request to frontend service
UI triggers /api call
Ingress routes /api → backend service
Backend returns JSON response
UI renders result dynamically
🧪 How to Run
minikube start --driver=docker
kubectl apply -f k8s/
kubectl get pods -n demo

Enable ingress:

minikube addons enable ingress

Map host:

echo "$(minikube ip) demo.local" | sudo tee -a /etc/hosts

Access:

http://demo.local
📊 Key Learnings
Kubernetes service discovery
Ingress routing rules
ConfigMap-based frontend injection
Microservice communication in K8s
Debugging multi-layer networking issues
📌 Author

Built as a Kubernetes learning + systems design demo project.


---

# 🧭 Architecture Diagram


— Clean mental model (for interviews)
User
  ↓
Ingress (demo.local router)
  ↓───────────────↓
Frontend        Backend API
(NGINX)        (http-echo)
  ↓               ↓
ConfigMap     JSON Response
<img width="2878" height="1752" alt="image" src="https://github.com/user-attachments/assets/c924aa1b-4f34-450b-981c-bca6c4cf5b6b" />
