# Instructions for Testing the ToDo Application

This guide provides explicit steps to test the application's availability using different Kubernetes networking methods.

---

### 1. Test via ClusterIP DNS (From Inside a Busybox Container)

To verify that the ClusterIP service correctly resolves via internal DNS and balances traffic, spin up a temporary interactive `busybox` pod:

```bash
kubectl run busybox --image=busybox -n todoapp -it --rm -- sh
```
# Once you are inside the container shell, execute the following explicit HTTP request to hit the service by its DNS name on port 80:

```bash
wget -qO- http://todoapp-service:80
```

# To test ToDo application using the service port-forward command. Run this command:

```bash
kubectl port-forward service/todoapp-service 8081:80 -n todoapp
```

# Once, port is forwarding you can reach this app following this URL in your browser:

http://localhost:8081


> **Note:** To test the application, open a browser and use the IP address of one of your cluster nodes combined with the configured NodePort.
>Since we are running a local development cluster without publicly accessible external IP addresses, you should use the concrete port 30007 defined in the NodePort manifest.
>Open your browser and navigate directly to:

http://localhost:30007
👉 