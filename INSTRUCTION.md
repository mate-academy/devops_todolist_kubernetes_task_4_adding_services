Apply the manifests to create the namespace, pods, and services
1 Create the namespace
kubectl apply -f namespace.yml


Вивід:

namespace/todoapp unchanged

2 Create the pods and services
Create the pods
kubectl apply -f todoapp-pod.yml


Вивід:

pod/todoapp-1 unchanged
pod/todoapp-2 unchanged

Create the ClusterIP service
kubectl apply -f clusterIP.yml


Вивід:

service/todoapp-servise created

Create the NodePort service
kubectl apply -f NodePort.yml


Вивід:

service/todoapp-nodeport-service unchanged

- Check the statuses of the pods
kubectl get pods -n todoapp


Вивід:

NAME        READY   STATUS    RESTARTS      AGE
todoapp-1   1/1     Running   6 (34m ago)   42m
todoapp-2   1/1     Running   6 (34m ago)   42m

- Check the services
kubectl get services
# або
kubectl get svc


Вивід:

NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   14d

3️⃣ Create a testing pod (busybox)
kubectl apply -f busybox.yml


Вивід:

pod/busybox created

4️⃣ Test connectivity to the ClusterIP service
-1- kubectl exec -it busybox -n todoapp -- /bin/sh


В середині pod:

curl http://todoapp-servise.todoapp.svc.cluster.local:80

-2- kubectl port-forward service/todoapp-nodeport-service 8081:80
Дізнайся IP-адресу ноди:

kubectl get nodes -o wide


У колонці INTERNAL-IP або EXTERNAL-IP буде адреса (наприклад 172.19.0.4).

Подивись порт, відкритий NodePort-сервісом:

kubectl get svc -n todoapp


У колонці PORT(S) знайди значення після двокрапки, наприклад 80:30080/TCP.
30080 — це порт, який відкрито зовні.

Відкрий застосунок у браузері:

http://<Node-IP>:30080
