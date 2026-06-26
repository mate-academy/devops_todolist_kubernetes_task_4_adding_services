# How to run TODO list with Kubernetes

### Clone repository with project

```bash
git clone https://github.com/maxmlv/devops_todolist_kubernetes_task_4_adding_services.git
cd devops_todolist_kubernetes_task_4_adding_services
```

## Apply Kubernetes manifests

### Create `todoapp` namespace

```bash
kubectl apply -f .infrastructure/namespace.yml
```

### Create a pod `busybox`

```bash
kubectl apply -f .infrastructure/busybox.yml
```

This pod runs a container from the image `ikulyk404/busyboxplus:curl`. This container is useful for sending requests to the TODO app via `curl`.

### Create a pod `todoapp`

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

This pod runs a container from the image `ikulyk404/todoapp:3.0.0` with readiness and liveness probes configured.

### Create ClusterIP service

```bash
kubectl apply -f .infrastructure/cluster-ip.yml
```

### Create NodePort service

```bash
kubectl apply -f .infrastructure/node-port.yml
```

## Namespace `todoapp` should look like this:

```
Pods:

NAME        READY   STATUS    RESTARTS      AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
busybox     1/1     Running   2 (10m ago)   44m   10.244.0.5   desktop-control-plane   <none>           <none>
todoapp-1   1/1     Running   0             44m   10.244.0.6   desktop-control-plane   <none>           <none>
todoapp-2   1/1     Running   0             44m   10.244.0.7   desktop-control-plane   <none>           <none>

Services:

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
todoapp-clusterip-service   ClusterIP   10.96.250.166   <none>        80/TCP         11m
todoapp-nodeport-service    NodePort    10.96.204.253   <none>        80:30007/TCP   40m
```

## How to test the application

### Check readiness and liveness probe responses from the app

You can see if the `todoapp` is ready for traffic and working properly. As an example we will use `todoapp-1` pod.

```bash
kubectl describe pod todoapp-1 -n todoapp
```
Look for the `Liveness` and `Readiness` lines in the output.

```
Containers:
  todoapp:
    Container ID:   containerd://654814d265020199672f113412d8208af05982da17c7167d988c5151b80d26f1
    Image:          ikulyk404/todoapp:3.0.0
    Image ID:       docker.io/ikulyk404/todoapp@sha256:f1254c1c05283baf2797c76b3e07c9e11f5707c8a363f279d5e2edb6013781c3
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 26 Jun 2026 10:07:33 +0300
    Ready:          True
    Restart Count:  0
--> Liveness:       http-get http://:8080/api/health delay=60s timeout=1s period=5s #success=1 #failure=3
--> Readiness:      http-get http://:8080/api/ready delay=5s timeout=1s period=5s #success=1 #failure=3
    Environment:
      PYTHONUNBUFFERED:  1
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-82j6d (ro)
```

### via Port-forward using ClusterIP

```bash
kubectl port-forward service/todoapp-clusterip-service 8081:80 -n todoapp
```
Now the application is available at `127.0.0.1:8081`.

### via `busybox` Pod using `curl`

For this, we will need to know the `todoapp` pod's IP address:

```bash
kubectl get pods -n todoapp -o wide
```

```
Output example:

NAME        READY   STATUS    RESTARTS       AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
busybox     1/1     Running   5 (7m3s ago)   90m   10.244.0.5   desktop-control-plane   <none>           <none>
todoapp-1   1/1     Running   0              90m   10.244.0.6   desktop-control-plane   <none>           <none>
todoapp-2   1/1     Running   0              90m   10.244.0.7   desktop-control-plane   <none>           <none>
```

Now we can connect to the `busybox` container.

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Inside `busybox`, we can send a GET request via `curl` using `ClusterIP` service' DNS name:

```bash
curl http://todoapp-clusterip-service.todoapp.svc.cluster.local
```
A successful response confirms the service is correctly routing to the todoapp pods.

### via NodePort

We have `NodePort` service listening on port `30007`

```
Services:

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
todoapp-clusterip-service   ClusterIP   10.96.250.166   <none>        80/TCP         11m
todoapp-nodeport-service    NodePort    10.96.204.253   <none>        80:30007/TCP   40m
```

The application should be available at `http://localhost:30007`