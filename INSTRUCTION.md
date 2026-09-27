# Kubernetes ToDo Application Instructions

This document explains how to deploy and test the ToDo application in Kubernetes.

The setup contains:

- two ToDo application pods;
- one BusyBox pod with `curl`;
- one `ClusterIP` service;
- one `NodePort` service;
- namespace `todoapp`.

---

## 1. Apply Kubernetes manifests

Create the namespace first:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Create the two ToDo application pods:

```bash
kubectl apply -f .infrastructure/pod.yaml
```

Create the BusyBox pod:

```bash
kubectl apply -f .infrastructure/busybox.yml
```

Create the ClusterIP service:

```bash
kubectl apply -f .infrastructure/service-clusterip.yaml
```

Create the NodePort service:

```bash
kubectl apply -f .infrastructure/service-nodeport.yaml
```

Check all pods:

```bash
kubectl get pods -n todoapp
```

Expected result:

```text
NAME             READY   STATUS    RESTARTS
busybox          1/1     Running   0
todolist-pod-1   1/1     Running   0
todolist-pod-2   1/1     Running   0
```

Check pod labels:

```bash
kubectl get pods -n todoapp --show-labels
```

Both application pods should have:

```text
app=todolist
```

Check services:

```bash
kubectl get services -n todoapp
```

Expected services:

```text
todolist-clusterip
todolist-nodeport
```

Check that the ClusterIP service sees both ToDo pods:

```bash
kubectl get endpoints todolist-clusterip -n todoapp
```

The output should contain two pod IP addresses with port `8000`.

---

## 2. Verify environment variables in the application pods

Check all environment variables in the first pod:

```bash
kubectl exec -n todoapp todolist-pod-1 -- printenv
```

Check individual values:

```bash
kubectl exec -n todoapp todolist-pod-1 -- printenv DB_ENGINE
kubectl exec -n todoapp todolist-pod-1 -- printenv DB_NAME
kubectl exec -n todoapp todolist-pod-1 -- printenv TIME_ZONE
kubectl exec -n todoapp todolist-pod-1 -- printenv LANGUAGE_CODE
```

Check the second pod:

```bash
kubectl exec -n todoapp todolist-pod-2 -- printenv
```

---

## 3. Test the application through ClusterIP DNS from BusyBox

The `ClusterIP` service is available inside the Kubernetes cluster by its DNS name:

```text
todolist-clusterip
```

Enter the BusyBox pod:

```bash
kubectl exec -it -n todoapp busybox -- sh
```

Inside BusyBox, call the application through the ClusterIP service DNS name:

```bash
curl http://todolist-clusterip:8000/
```

The command should return the HTML of the ToDo application.

Test the readiness endpoint:

```bash
curl http://todolist-clusterip:8000/api/readiness/
```

Expected response:

```text
OK
```

Test the liveness endpoint:

```bash
curl http://todolist-clusterip:8000/api/liveness/
```

Expected response:

```text
OK
```

Exit BusyBox:

```bash
exit
```

The ClusterIP service selects both pods by the label:

```text
app=todolist
```

Therefore requests sent to `todolist-clusterip` can be forwarded to either `todolist-pod-1` or `todolist-pod-2`.

---

## 4. Test the ToDo application using service port-forward

Forward local port `8000` to the ClusterIP service:

```bash
kubectl port-forward -n todoapp service/todolist-clusterip 8000:8000
```

Expected output:

```text
Forwarding from 127.0.0.1:8000 -> 8000
Forwarding from [::1]:8000 -> 8000
```

Keep this terminal open.

Open another terminal and test the application:

```bash
curl http://localhost:8000/
```

Or open it in a browser:

```text
http://localhost:8000/
```

Test readiness:

```bash
curl http://localhost:8000/api/readiness/
```

Expected response:

```text
OK
```

Test liveness:

```bash
curl http://localhost:8000/api/liveness/
```

Expected response:

```text
OK
```

Stop port-forward with:

```text
Ctrl+C
```

---

## 5. Access the application using the NodePort service

Check the NodePort service:

```bash
kubectl get service todolist-nodeport -n todoapp
```

The service exposes the application on NodePort:

```text
30080
```

With Docker Desktop, test it from the host machine:

```bash
curl http://localhost:30080/
```

Or open it in a browser:

```text
http://localhost:30080/
```

You can also test the health endpoints:

```bash
curl http://localhost:30080/api/readiness/
curl http://localhost:30080/api/liveness/
```

Expected response from both health endpoints:

```text
OK
```

---

## 6. Useful troubleshooting commands

Show pods with IP addresses:

```bash
kubectl get pods -n todoapp -o wide
```

Show services:

```bash
kubectl get services -n todoapp
```

Show service endpoints:

```bash
kubectl get endpoints -n todoapp
```

Describe the first application pod:

```bash
kubectl describe pod todolist-pod-1 -n todoapp
```

Describe the second application pod:

```bash
kubectl describe pod todolist-pod-2 -n todoapp
```

Show logs from the first application pod:

```bash
kubectl logs -n todoapp todolist-pod-1
```

Show logs from the second application pod:

```bash
kubectl logs -n todoapp todolist-pod-2
```

---

## 7. Delete all created Kubernetes resources

The easiest way to remove all resources created for this task is to delete the namespace:

```bash
kubectl delete namespace todoapp
```

This removes the pods and services inside the `todoapp` namespace.

---

## 8. Create a Pull Request

Create a new branch for the task:

```bash
git switch -c kubernetes-services
```

Check changed files:

```bash
git status
```

Add the required files:

```bash
git add .
```

Create a commit:

```bash
git commit -m "Add Kubernetes services and pod configuration"
```

Push the branch to your fork:

```bash
git push -u origin kubernetes-services
```

Open GitHub and create a Pull Request from:

```text
kubernetes-services
```

into the original repository branch:

```text
main
```

Suggested Pull Request title:

```text
Add Kubernetes services and pod configuration
```

Suggested Pull Request description:

```markdown
## Changes

- Added a second ToDo application pod
- Added `app: todolist` labels to both application pods
- Added a ClusterIP service for traffic balancing between both pods
- Added a NodePort service for access on the node level
- Added container environment variables to the pod manifest
- Added instructions for ClusterIP DNS testing from BusyBox
- Added instructions for service port-forward testing
- Added instructions for NodePort access
```

After creating the Pull Request, attach its URL to the Mate Academy task for validation.
