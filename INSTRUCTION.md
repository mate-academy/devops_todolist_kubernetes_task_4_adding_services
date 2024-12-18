First of all set the namespace to `todoapp` as the default:

  ```bash
  kubectl config set-context --current --namespace=todoapp
  ```
---
### 1 Test the ClusterIP Service from a BusyBox Container

- Exec into the BusyBox pod:

  ```bash
  kubectl exec -it busybox -- sh
  ```

- Use `curl` to test the ClusterIP service:

  ```bash
  curl todoapp-cluster-ip.todoapp.svc.cluster.local
  ```
---
### 2 Test the Application with Port Forwarding

- To forward the port from the local machine to the pod:

  ```bash
  kubectl port-forward pod/todoapp 8080:8080
  ```

- Access the ToDo application at:

  ```bash
  http://localhost:8080
  ```
---
### 3 Access the App Using NodePort

- Access the ToDo application from any node in your Kubernetes cluster using the NodePort:

  ```bash
  http://localhost:30303
  ```
