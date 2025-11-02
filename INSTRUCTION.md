# Запусти busybox у потрібному namespace (наприклад, todoapp):
kubectl run -it --rm busybox --image=busybox --restart=Never -n todoapp -- sh
# У консолі busybox виконай:
wget -qO- http://todoapp-clusterip-service:8000


# Запусти порт-форвард на сервіс:
kubectl port-forward svc/todoapp-clusterip-service 8000:8000 -n todoapp
# Відкрий у браузері:
http://localhost:8000


# Доступ через NodePort сервіс. Подивись IP ноди:
kubectl get nodes -o wide
# Подивись NodePort порт:
kubectl get svc todoapp-nodeport-service -n todoapp
# Відкрий у браузері:
http://<NODE_IP>:<NODE_PORT>