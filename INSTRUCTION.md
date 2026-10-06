# TodoApp Kubernetes Testing

## 1. ClusterIP

Start a BusyBox container:

```bash
kubectl run busybox --image=busybox:1.36 --restart=Never -n todoapp --rm -it -- sh
```

Call the TodoApp through the ClusterIP Service DNS:

```bash
wget -qO- http://<SERVICE-NAME>:<SERVICE-PORT>/api/health
```

Exit BusyBox:

```bash
exit
```

## 2. Port-forward

Forward the Service port to your local machine:

```bash
kubectl port-forward service/<SERVICE-NAME> <LOCAL-PORT>:<SERVICE-PORT> -n todoapp
```

Then open:

```text
http://localhost:<LOCAL-PORT>
```

## 3. NodePort

Check the NodePort:

```bash
kubectl get service <SERVICE-NAME> -n todoapp
```

Get the Node IP:

```bash
kubectl get nodes -o wide
```

Open the application:

```text
http://<NODE-IP>:<NODE-PORT>
```

## Manifests

- `namespace.yml` — creates the `todoapp` namespace.
- `todoapp-pod.yml` — deploys two TodoApp pods with the label `app: todolist`.
- `clusterip.yml` — exposes the two pods through a ClusterIP Service.
- `nodeport.yml` — exposes the application through a NodePort Service.
- `busybox.yml` — provides a BusyBox pod for testing the ClusterIP Service.
