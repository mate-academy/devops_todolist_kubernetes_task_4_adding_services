# INSTRUCTION

## 1. Apply Kubernetes manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todoapp-pod-2.yml
kubectl apply -f .infrastructure/todoapp-service.yml
kubectl apply -f .infrastructure/todoapp-nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
```

## 2. Test ClusterIP Service from BusyBox

Open a shell inside the BusyBox pod:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Send a request to the application using the Service DNS name:

```bash
curl http://todoapp-service:8080/api/ready
```

Expected response:

```text
Readiness OK
```

Exit the BusyBox shell:

```bash
exit
```

## 3. Test the Service using port-forward

Run:

```bash
kubectl port-forward service/todoapp-service 8080:8080 -n todoapp
```

In another terminal, execute:

```bash
curl http://localhost:8080/api/ready
```

Expected response:

```text
Readiness OK
```

## 4. Test the application using NodePort

Get the Kubernetes node IP:

```bash
kubectl get nodes -o wide
```

Open the application using the node IP and NodePort `30080`:

```text
http://<NODE-IP>:30080/api/ready
```

Alternatively, try:

```text
http://localhost:30080/api/ready
```

Expected response:

```text
Readiness OK
```

**Note:** NodePort accessibility depends on the local Kubernetes environment and its network configuration.