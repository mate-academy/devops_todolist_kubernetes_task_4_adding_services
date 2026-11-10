# ToDo app on Kubernetes: how to deploy and test

All resources live in the `todoapp` namespace. Two pods (`todoapp` and `todoapp-1`) carry the label `app: todolist`, and both services select that label.

| Resource | Name | Port mapping |
|---|---|---|
| ClusterIP service | `todoapp-service` | `80` -> pod `8080` |
| NodePort service | `todoapp-nodeport-service` | node `30007` -> service `80` -> pod `8080` |

## 1. Deploy

Apply the manifests in this order (the namespace must exist first):

```bash
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f clusterIp.yml
kubectl apply -f nodePort.yml
kubectl apply -f busybox.yml
```

Check that the pods are `Running` and `READY 1/1`, and that each service has two endpoints (one per pod):

```bash
kubectl get pods -n todoapp --show-labels
kubectl get svc -n todoapp
kubectl get endpoints -n todoapp
```

If the endpoints list is empty, the pods are not ready yet or the labels do not match the service selector.

## 2. Test the app by calling the ClusterIP service DNS from a busybox container

Inside the cluster, the ClusterIP service is reachable by its DNS name:

```
todoapp-service.todoapp.svc.cluster.local
```

From a pod in the same namespace, the short name `todoapp-service` also works. The service listens on port `80`, which is the default HTTP port, so no port number is needed in the URL.

Wait until the busybox pod is ready, then call the service from it:

```bash
kubectl wait --for=condition=Ready pod/busybox -n todoapp --timeout=60s

kubectl exec -it busybox -n todoapp -- curl http://todoapp-service.todoapp.svc.cluster.local
```

You can also open a shell in the busybox container and run the commands there:

```bash
kubectl exec -it busybox -n todoapp -- sh

curl http://todoapp-service
curl http://todoapp-service.todoapp.svc.cluster.local/api/
nslookup todoapp-service
```

A successful call returns the HTML of the landing page (or the API output for `/api/`).

To see the traffic being balanced between the two pods, send several requests and then compare the logs of both pods. Each pod should show some of the requests:

```bash
for i in 1 2 3 4 5 6; do
  kubectl exec busybox -n todoapp -- curl -s -o /dev/null -w "%{http_code}\n" http://todoapp-service/api/
done

kubectl logs todoapp -n todoapp --tail=10
kubectl logs todoapp-1 -n todoapp --tail=10
```

## 3. Test the app using the service `port-forward` command

Forward a local port to the ClusterIP service (local `8080` -> service port `80`):

```bash
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
```

Keep the command running and, in a browser or a second terminal, open:

- Landing page: http://localhost:8080/
- API: http://localhost:8080/api/

```bash
curl http://localhost:8080/
curl http://localhost:8080/api/
```

Stop the forwarding with `Ctrl+C`. If local port 8080 is already in use, pick another one, for example `8081:80`, and use `http://localhost:8081/`.

Note: `port-forward` to a service connects to one pod behind it, so it does not demonstrate load balancing.

## 4. Access the app using the NodePort service

The NodePort service exposes the app on port `30007` of every cluster node.

### Minikube

```bash
minikube service todoapp-nodeport-service -n todoapp --url
```

The command prints a URL such as `http://192.168.49.2:30007`. With the Docker driver on macOS or Windows, it opens a tunnel and prints a `127.0.0.1` URL instead. Keep that terminal open while you use it. Then call it:

```bash
curl <printed-url>/
curl <printed-url>/api/
```

If the node IP is reachable from your machine, you can also use it directly:

```bash
minikube ip
curl http://$(minikube ip):30007/
```

### Other clusters (kind, k3s, cloud, bare metal)

Find a node IP and use it with port `30007`:

```bash
kubectl get nodes -o wide
curl http://<NODE-IP>:30007/
```

Make sure that port `30007` is allowed by the firewall or security group. With kind, node ports are not published to the host unless `extraPortMappings` is configured; use `port-forward` instead.

## 5. Check the container environment variables

The environment variables are set in `todoapp-pod.yml` under `spec.containers[].env`. To verify them in a running pod:

```bash
kubectl exec todoapp -n todoapp -- env | grep PYTHONUNBUFFERED
```

## 6. Cleanup

```bash
kubectl delete namespace todoapp
```
