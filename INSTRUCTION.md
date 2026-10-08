# Instructions for Deploying and Testing Multi-Pod ToDo Application

## 1. How to Apply Manifests
Apply all manifests from the root directory:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pods.yml
kubectl apply -f .infrastructure/todoapp-clusterip.yml
kubectl apply -f .infrastructure/todoapp-nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
```

---

## 2. Test ClusterIP via Service DNS from BusyBox
Enter the busybox container and curl the ClusterIP service using its full FQDN:

```bash
kubectl exec -it busybox-curl -n todoapp -- sh

# Inside busybox, call the full valid service DNS name
curl http://cluster.local
```

---

## 3. Test ToDo Application using Service Port-Forward
Forward local port to the Kubernetes Service instead of a single pod:

```bash
kubectl port-forward svc/todoapp-clusterip 8000:80 -n todoapp
```
Now access it via browser: http://127.0.0.1:8000

---

## 4. Access App via NodePort Service
```bash
minikube service todoapp-nodeport -n todoapp --url
```
