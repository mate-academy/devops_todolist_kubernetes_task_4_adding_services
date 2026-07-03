
# Переконайтеся, що под busybox успішно запущено: 
kubectl get pods -n todoapp
# Підключіться до інтерактивного терміналу цього поду:
kubectl exec -it busybox -n todoapp -- sh
# ВСЕРЕДИНІ ПОДУ: Виконайте HTTP-запит через wget на корінь додатка:
wget -qO- http://todo-clusterip-service:80/

# Запустіть тунелювання порту з локального 8000 на 80 порт сервісу
kubectl port-forward svc/todo-clusterip-service 8000:80 -n todoapp
# В іншому терміналі:
curl http://localhost:8000/


# Запустіть тунелювання конкретно для NodePort сервісу на зовнішній порт 30008
kubectl port-forward svc/todo-nodeport-service 30008:80 -n todoapp

# Тепер відкрийте браузер на комп'ютері або виконайте curl у новому терміналі:
curl http://localhost:30008/