# ToDo App Kubernetes Testing Guide

## 1. Apply all manifests

```bash
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f todoapp-pod-2.yml
kubectl apply -f service-clusterip.yml
kubectl apply -f service-nodeport.yml
kubectl apply -f busybox.yml
```

---

## 2. Test ClusterIP Service using Busybox (DNS + request)

Run busybox container:

```bash
kubectl run -it --rm busybox --image=busybox --restart=Never -- sh
```

Inside the container:

```sh
nslookup todoapp-clusterip
wget -qO- http://todoapp-clusterip/api/
```

This verifies:

- DNS resolution inside Kubernetes cluster
- Service routing to Pods

---

## 3. Test application using port-forward

Run port-forward:

```bash
kubectl port-forward svc/todoapp-clusterip 8080:80
```

Then open in browser:

```
http://localhost:8080
```

This allows local access to the Kubernetes service.

---

## 4. Test application using NodePort service

Get node IP address:

```bash
kubectl get nodes -o wide
```

Then open in browser:

```
http://<node-ip>:30080
```

This exposes the application outside the Kubernetes cluster.

---

## 5. Verify resources

Check pods:

```bash
kubectl get pods -n todoapp
```

Check services:

```bash
kubectl get svc -n todoapp
```