# INSTRUCTION

## Apply manifests

Apply all manifests:

```
kubectl apply -f .infrastructure/
```

## Test using ClusterIP Service DNS from BusyBox

Open the BusyBox container:

```
kubectl exec -it busybox -n todoapp -- sh
```

Call the application using the ClusterIP Service DNS:

```
curl http://todoapp-clusterip:8080/
```

## Test using Service port-forward

Forward the ClusterIP Service port:

```
kubectl port-forward service/todoapp-clusterip 8080:8080 -n todoapp
```

Then open:

```
http://localhost:8080/
```

## Access the application using NodePort Service

Get the Kubernetes node IP:

```
kubectl get nodes -o wide
```

The NodePort is `30080`.

Access the application at:

```
http://<NODE-IP>:30080/
```
