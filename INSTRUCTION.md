# ToDo Application - Kubernetes Testing Instructions

The ToDo application is deployed in the `todoapp` namespace.

The deployment contains two ToDo application Pods:

- `todoapp`
- `todoapp-1`

Both Pods have the following label:

```yaml
app: todolist
```

The application is exposed using two Kubernetes Services:

- `todoapp-service` - ClusterIP Service
- `todoapp-nodeport-service` - NodePort Service

The application container listens on port `8080`.

---

## 1. Check the Pods:

```bash
kubectl get pods -n todoapp
```

## 1.2 Check the Services:

```bash
kubectl get services -n todoapp
```

## 2. Test the application using ClusterIP Service DNS from a BusyBox container

```bash
kubectl run busybox --image=busybox:latest --restart=Never -n todoapp -- sleep 3600
```

## 2.2 Check that the BusyBox Pod is running:

```bash
kubectl get pods -n todoapp
```

## 2.3 Open a shell inside the BusyBox container:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

## 3. Test the ToDo application using Service port-forward

```bash
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
```

The application should be opened in a web browser:

```text
http://localhost:8080
```

## 4. Access the application using NodePort Service

The NodePort configured for the application is:

```text
30007
```

Get the IP address of the Kubernetes Node:

```bash
kubectl get nodes -o wide
```

Find the value in the `INTERNAL-IP` column.

The application can then be accessed using, open the following address in a web browser:

```text
http://<NODE-IP>:30007
```

Replace `<NODE-IP>` with the actual IP address of the Kubernetes Node.
