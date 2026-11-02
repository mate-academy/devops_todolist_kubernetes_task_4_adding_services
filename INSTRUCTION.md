# ToDo app: Services in Kubernetes

## 1. Apply the manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

Check that both ToDo pods (`todoapp` and `todoapp-2`) are `1/1 Running` and that the services exist:

```bash
kubectl get pods -n todoapp -o wide --show-labels
kubectl get svc -n todoapp
kubectl get endpoints todoapp-clusterip -n todoapp
```

The `endpoints` command should list the IPs of both ToDo pods: this is how the ClusterIP service balances traffic between them.

## 2. Test via ClusterIP service DNS from busybox

```bash
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip.todoapp.svc.cluster.local/api/health
```

Short name also works from the same namespace:

```bash
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip/api/ready
```

Expected answers: `Health OK` and `Readiness OK`.

## 3. Test via service port-forward

```bash
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

In another terminal:

```bash
curl http://localhost:8080/api/health
```

Or open http://localhost:8080 in a browser. Stop port-forward with `Ctrl + C`.

## 4. Access via NodePort service

The NodePort service exposes the app on port `30007` of every node.

Get the node IP:

```bash
kubectl get nodes -o wide
```

Open the app at `http://<NODE_INTERNAL_IP>:30007`. On Docker Desktop with the kubeadm cluster the node is available as `localhost`:

```bash
curl http://localhost:30007/api/health
```

You can also check the NodePort from inside the cluster:

```bash
kubectl exec -it busybox -n todoapp -- curl http://<NODE_INTERNAL_IP>:30007/api/health
```
