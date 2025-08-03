Тестування через busybox:
kubectl run -it --rm busybox --image=busybox --restart=Never -- sh
wget -qO- http://todo-clusterip:8000

Port-forward:
kubectl port-forward service/todo-clusterip 8080:8000

Потім перейти у браузері на http://localhost:8080

Тест NodePort:
Використовуй IP-адресу ноди:

curl http://<NodeIP>:30080
або в браузері — http://<NodeIP>:30080