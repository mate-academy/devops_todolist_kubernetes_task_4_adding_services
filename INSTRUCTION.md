## 1. Тестування через ClusterIP DNS (busybox)
Створити busybox pod:
```bash
kubectl run busybox --image=busybox --rm -it -- sh
Всередині виконати запит до сервісу:
wget -qO- http://todoapp-service:8000
2. Тестування через port-forward
Запустити port-forward:
kubectl port-forward svc/todoapp-service 8000:8000
Відкрити в браузері:
http://localhost:8000

3. Доступ через NodePort
Отримати IP ноди:
kubectl get nodes -o wide
Відкрити в браузері:
http://<NODE_IP>:32049
