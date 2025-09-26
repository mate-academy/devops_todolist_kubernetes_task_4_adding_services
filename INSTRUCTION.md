## 1. Test ToDo Application via ClusterIP Service

1. Start a temporary BusyBox pod (already exists in our case):

```bash
kubectl exec -n todoapp -it busybox -- sh
```

2. Inside the BusyBox pod, call the ToDo app service using `curl`:

```sh
curl http://todo-cluster-ip
```

You should see the response from the ToDo app, which confirms the ClusterIP service is routing traffic to one of the pods.

---

## 2. Test ToDo Application via Port-Forward

1. Forward the service port to your local machine:

```bash
kubectl port-forward -n todoapp svc/todo-cluster-ip 8080:80
```

2. Access the ToDo app in your browser or via `curl`:

```bash
curl http://localhost:8080
```

You should see the ToDo app’s landing page or API response.

---

## 3. Test ToDo Application via NodePort Service

1. Check the NodePort service:

```bash
kubectl get svc todo-node-port -n todoapp
```

2. Access the ToDo app from **inside the cluster** (e.g., using BusyBox pod):

```bash
kubectl exec -n todoapp -it busybox -- sh
curl http://todo-node-port:80
```

> Even though it’s a NodePort service, you can still access it via the service name inside the cluster without needing the node IP.

3. Alternatively, forward the NodePort to your local machine:

```bash
kubectl port-forward -n todoapp svc/todo-node-port 8081:80
```

http://localhost:8081

4. Alternatively, you can access NodePort directly:

http://localhost:30007

This confirms that the NodePort service is routing traffic correctly, both inside and outside the cluster.
