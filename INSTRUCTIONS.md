# Kubernetes: deploy & verify TodoApp

## Apply all manifests

kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/clusterIP.yml
kubectl apply -f .infrastructure/nodeport.yml

---

## Test service connectivity from inside the cluster (BusyBox)

kubectl exec -it busybox -n todoapp -- sh
curl http://todoapp-service
exit

---

## Access service locally via port-forward

kubectl port-forward service/todoapp-service 8089:80 -n todoapp

---

## Access service via NodePort

# Open in browser:

# http://localhost:30007
