# Kubernetes ToDo App — Testing Instructions

## 📌 Namespace
All resources are deployed in the `todoapp` namespace.

---

## ✅ 1. Test app via ClusterIP DNS from busybox pod

1. Make sure the service `todoapp-clusterip` is created:

   ```bash
   kubectl get svc -n todoapp
Output should include something like:

css
todoapp-clusterip   ClusterIP   10.x.x.x   <none>   80/TCP   ...
Open an interactive session in the BusyBox pod:

bash
kubectl -n todoapp exec -it busybox -- sh
Inside BusyBox, call the app through the ClusterIP DNS:

sh
wget -qO- http://todoapp-clusterip.todoapp.svc.cluster.local/api/
You should receive a JSON response from the Django API.

🌐 2. Test via NodePort from local browser or curl
Check that the NodePort service exists:

bash
kubectl get svc -n todoapp
You should see:

yaml
todoapp-nodeport   NodePort   ...   PORT: 80 → 8080, NODEPORT: 30007
Open your browser (or PowerShell) and visit:

bash
http://localhost:30007/api/
You should see a valid JSON with /users/, /todos/, /todolists/.

🌍 3. Test ExternalName access through /external-call
Visit the NodePort-based route in your browser:

bash
Копировать
Редактировать
http://localhost:30007/external-call/
Expected result:

A JSON containing data returned from http://httpbin.org/get via DNS service httpbin-api.todoapp.svc.cluster.local.

📦 Image Used
Docker image for app:
ikulyk404/todoapp:3.0.0
Docker image for BusyBox with curl:
ikulyk404/busyboxplus:curl

