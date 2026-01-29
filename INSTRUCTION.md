
Перед тестуванням застосуйте всі маніфести: `kubectl apply -f todoapp-pod.yaml` (2 Pod ToDo) , `kubectl apply -f ClusterIP.yml` (Service ClusterIP), `kubectl apply -f NodePort.yml` (Service NodePort) та `kubectl apply -f busybox.yaml` (Pod busybox).

Тестування через DNS-сервіс ClusterIP з busybox: `kubectl exec -it busybox -n todoapp -- sh` і виконайте запит `curl http://todoapp-service:80` (за потреби: `curl http://todoapp-service:80/api/health` та `curl http://todoapp-service:80/api/ready`).

Тестування через port-forward сервісу ClusterIP: `kubectl port-forward -n todoapp service/todoapp-service 8080:80`, перевірка: `curl http://localhost:8080`.

Доступ через NodePort: отримайте IP ноди `kubectl get nodes -o wide` і звертайтесь `curl http://<NODE_IP>:30007` (або відкрийте `http://<NODE_IP>:30007` у браузері).

