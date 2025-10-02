# Chat App Kubernetes Deployment

This repository contains Kubernetes manifests to deploy the **Chat App** demo application. The app includes backend, frontend, MongoDB, and other microservices, with proper services, persistent storage, and ingress configuration.

---

## Prerequisites

* Kubernetes cluster (Minikube, Kind, or cloud)
* `kubectl` installed
* Optional: `ingress-nginx` controller for ingress routing
* Docker installed if building custom images

---

## Folder Structure

```
k8s/
├── backend-deployment.yaml
├── frontend-deployment.yaml
├── mongodb-deployment.yaml
├── persistent-volume.yaml
├── persistent-volume-claim.yaml
├── service.yaml
├── ingress.yaml
└── README.md
```

---

## Deployment Steps

### 1. Create Namespace

```bash
kubectl create namespace chat-app
```

### 2. Apply Persistent Volumes & Claims

```bash
kubectl apply -f persistent-volume.yaml -n chat-app
kubectl apply -f persistent-volume-claim.yaml -n chat-app
```

### 3. Deploy MongoDB

```bash
kubectl apply -f mongodb-deployment.yaml -n chat-app
```

### 4. Deploy Backend and Frontend

```bash
kubectl apply -f backend-deployment.yaml -n chat-app
kubectl apply -f frontend-deployment.yaml -n chat-app
```

### 5. Deploy Services

```bash
kubectl apply -f service.yaml -n chat-app
```

### 6. Deploy Ingress (Optional)

```bash
kubectl apply -f ingress.yaml -n chat-app
```

---

## Accessing the Application

1. Add to your `/etc/hosts` (Linux/Mac) or `C:\Windows\System32\drivers\etc\hosts` (Windows):

```
127.0.0.1 chatapp.com
```

2. Open the browser:

```
http://chatapp.com/
```

---

## Checking Resources

* **Pods**

```bash
kubectl get pods -n chat-app
```

* **Services**

```bash
kubectl get svc -n chat-app
```

* **Ingress**

```bash
kubectl get ingress -n chat-app
```

* **Persistent Volumes**

```bash
kubectl get pv -n chat-app
kubectl get pvc -n chat-app
```

---

## Troubleshooting

* **Check pod logs**

```bash
kubectl logs <pod-name> -n chat-app
```

* **Check events**

```bash
kubectl get events -n chat-app
```

* **Common Issues**

  * Missing environment variables (like `MONGODB_URI`)
  * Port conflicts (`sudo lsof -i:<port>`)
  * Ingress not reachable (check `ingress-nginx` controller)

---

## Notes

* Ensure all environment variables in deployment manifests are correct.
* For LoadBalancer services in Minikube, run:

```bash
minikube tunnel
```

* For local development, `ClusterIP` services are accessible via port-forwarding:

```bash
kubectl port-forward svc/<service-name> <local-port>:<service-port> -n chat-app
```
