# Task 4: Adding services

## 1. Deploy the application

Apply the manifests in this order (the namespace must exist first):

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todoapp-pod-2.yml
kubectl apply -f .infrastructure/clusterip-service.yml
kubectl apply -f .infrastructure/nodeport-service.yml
```

There are two identical ToDo pods with different names (`todoapp` and `todoapp-2`,
files `todoapp-pod.yml` and `todoapp-pod-2.yml`). Both have the label `app: todolist`,
which both services use as a selector.

Check that all pods are `Running` and that each service has two endpoints:

```
kubectl get pods -n todoapp
kubectl get svc -n todoapp
kubectl get endpoints -n todoapp
```

## 2. Test the ClusterIP service from the busybox pod (DNS)

The `busybox` pod uses the image `ikulyk404/busyboxplus:curl`, which already
includes `curl` (the plain `busybox` image does not). Open a shell in the pod:

```
kubectl exec -it busybox -n todoapp -- sh
```

Inside the pod, request the ToDo landing page through the service DNS name:

```
curl -i http://todoapp-clusterip.todoapp/
```

Expected result: `HTTP/1.1 200 OK` and the HTML of the ToDo application
("Dead simple Todolists"). The health endpoint can be checked as well:

```
curl -i http://todoapp-clusterip.todoapp/api/health
```

If `curl` is not available in your busybox image, use `wget` instead:

```
wget -qO- http://todoapp-clusterip.todoapp/
```

Leave the pod shell with `exit`.

The service DNS name is `<service>.<namespace>`
(full form: `todoapp-clusterip.todoapp.svc.cluster.local`).

## 3. Test the service with port-forward

```
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

Then open the ToDo landing page in the browser:

```
http://localhost:8080/
```

Expected result: the ToDo application page ("Dead simple Todolists").
The health endpoint `http://localhost:8080/api/health` returns `Health OK`.
Stop the forwarding with `Ctrl + C`.

## 4. Access the application through the NodePort service

The NodePort service `todoapp-nodeport` exposes the app on port `30080` of the node.
With Docker Desktop Kubernetes the node is the local machine, so open the landing page:

```
http://localhost:30080/
```

Expected result: the ToDo application page ("Dead simple Todolists").
The health endpoint `http://localhost:30080/api/health` returns `Health OK`.
