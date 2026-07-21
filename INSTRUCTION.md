# ToDo App — Kubernetes Services Instructions

## Project Structure

All Kubernetes manifests are located in the `.infrastructure` folder:

- `todoapp-pod.yml` — creates ToDo application pods
- `clusterip.yml` — creates a ClusterIP service that load-balances traffic between the two pods
- `nodeport.yml` — creates a NodePort service that exposes the app at the node level

## 1. Apply the manifests

Apply the manifests in the following order:

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterip.yml
kubectl apply -f .infrastructure/nodeport.yml
```

Check that all pods are running:

```bash
kubectl get pods -n todoapp
```

Check that the services were created:

```bash
kubectl get svc -n todoapp
```

## 2. Test the app using the service `port-forward` command

Forward a local port to the ClusterIP service (not directly to a pod):

```bash
kubectl port-forward -n todoapp service/todoapp-clusterip-svc 8000:80
```

Keep this command running in a terminal, then open your browser and navigate to:

http://localhost:8000

To stop port-forwarding, press `Ctrl+C` in the terminal.

## 3. Access the app using the NodePort service

Find the node port assigned to the NodePort service:

```bash
kubectl get svc todoapp-nodeport-svc -n todoapp
```

Look for the port mapping in the `PORT(S)` column, for example `8000:30000/TCP` —
`30000` is the node port.

Get the node's IP address (for local clusters like Kind or Minikube, this is
often `localhost` or the Docker host IP):

```bash
kubectl get nodes -o wide
```

Open your browser and navigate to:

http://<NODE_IP>:<NODE_PORT>

For example: http://localhost:30000

## Cleaning up

To remove all resources created by these manifests:

```bash
kubectl delete -f .infrastructure/nodeport.yml
kubectl delete -f .infrastructure/clusterip.yml
kubectl delete -f .infrastructure/todoapp-pod.yml
```

## Notes

- Both pods are labeled `app: todolist`, which the services use as a selector
  to route traffic to either pod.
- Environment variables for the containers are defined directly in the pod
  manifests.