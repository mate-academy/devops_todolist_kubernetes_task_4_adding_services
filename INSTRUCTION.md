## Testing

### 1. Testing the app via busybox (ClusterIP DNS)

First, launch the busybox pod:

```bash
kubectl apply -f busybox.yml
```

Open a shell inside it:

```bash
kubectl -n todoapp exec -it busybox -- sh
```

Then call the ClusterIP service by its DNS name:

```bash
wget -qO- http://todoapp-service.todoapp.svc.cluster.local
```

The port defaults to 80, so it doesn't need to be specified here — the service listens on 80. If the service used a different `port`, you would append it to the URL (e.g. `...svc.cluster.local:8080`).

### 2. Testing the service via port-forward

```bash
kubectl port-forward service/todoapp-service 8081:80 -n todoapp
```

Note: this forwards to the **service**, not a pod.

Then open `http://localhost:8081` in your browser to verify.

### 3. Accessing the app via the NodePort service

Get the access URL:

```bash
minikube service todoapp-service-nodeport -n todoapp --url
```

Copy the URL shown in the terminal and open it in a browser or with `curl`.