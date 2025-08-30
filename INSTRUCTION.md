# Инструкции по тестированию ToDo приложения в Kubernetes

## Предварительная подготовка

1. Убедитесь, что у вас запущен Kubernetes кластер
2. Установите kubectl и настройте доступ к кластеру

## Развертывание приложения

### 1. Создание namespace и подов

```bash
# Создание namespace
kubectl apply -f .infrastructure/namespace.yml

# Развертывание подов
kubectl apply -f .infrastructure/todoapp-pod.yml

# Проверка статуса подов
kubectl get pods -n todoapp
```

### 2. Создание сервисов

```bash
# Создание ClusterIP сервиса
kubectl apply -f .infrastructure/todoapp-clusterip-service.yml

# Создание NodePort сервиса
kubectl apply -f .infrastructure/todoapp-nodeport-service.yml

# Проверка сервисов
kubectl get services -n todoapp
```

### 3. Развертывание busybox для тестирования

```bash
kubectl apply -f .infrastructure/busybox.yml
```

## Тестирование приложения

### 1. Тестирование ClusterIP сервиса через busybox контейнер

Подключитесь к busybox контейнеру и протестируйте доступ к ClusterIP сервису:

```bash
# Подключение к busybox
kubectl exec -it busybox -n todoapp -- /bin/sh

# Тестирование доступа к ClusterIP сервису через DNS
curl http://todoapp-clusterip-service.todoapp.svc.cluster.local:8080

# Тестирование API endpoint
curl http://todoapp-clusterip-service.todoapp.svc.cluster.local:8080/api/

# Проверка health endpoint
curl http://todoapp-clusterip-service.todoapp.svc.cluster.local:8080/api/health

# Выход из контейнера
exit
```

### 2. Тестирование с помощью port-forward

Используйте port-forward для доступа к приложению через ClusterIP сервис:

```bash
# Port-forward для ClusterIP сервиса
kubectl port-forward service/todoapp-clusterip-service 8080:8080 -n todoapp

# В другом терминале протестируйте:
curl http://localhost:8080
curl http://localhost:8080/api/
curl http://localhost:8080/api/health

# Остановите port-forward (Ctrl+C)
```

### 3. Тестирование NodePort сервиса

NodePort сервис доступен на всех нодах кластера на порту 30080:

```bash
# Получите IP любой ноды кластера
kubectl get nodes -o wide

# Используйте IP ноды для доступа к приложению
curl http://<NODE_IP>:30080
curl http://<NODE_IP>:30080/api/
curl http://<NODE_IP>:30080/api/health

# Для локального кластера (minikube/kind):
curl http://localhost:30080
curl http://localhost:30080/api/
```

### 4. Тестирование балансировки нагрузки

Проверьте, что трафик балансируется между двумя подами:

```bash
# Просмотр логов первого пода
kubectl logs todoapp-1 -n todoapp -f

# В другом терминале просмотр логов второго пода
kubectl logs todoapp-2 -n todoapp -f

# В третьем терминале отправьте несколько запросов
for i in {1..10}; do
  curl http://localhost:8080/api/
  sleep 1
done
```

## Полезные команды для диагностики

```bash
# Проверка статуса всех ресурсов в namespace
kubectl get all -n todoapp

# Описание пода (для диагностики проблем)
kubectl describe pod todoapp-1 -n todoapp

# Описание сервиса
kubectl describe service todoapp-clusterip-service -n todoapp

# Просмотр логов пода
kubectl logs todoapp-1 -n todoapp

# Проверка endpoints сервиса
kubectl get endpoints -n todoapp

# Удаление всех ресурсов (при необходимости)
kubectl delete namespace todoapp
```

## Ожидаемые результаты

1. **ClusterIP сервис**: Доступен только внутри кластера через DNS имя `todoapp-clusterip-service.todoapp.svc.cluster.local:8080`
2. **NodePort сервис**: Доступен извне кластера через `<NODE_IP>:30080`
3. **Балансировка нагрузки**: Запросы должны распределяться между подами `todoapp-1` и `todoapp-2`
4. **Health checks**: Endpoints `/api/health` и `/api/ready` должны возвращать успешный ответ
5. **API**: Endpoint `/api/` должен возвращать JSON с информацией о API

## Устранение неполадок

- Если поды не запускаются, проверьте логи: `kubectl logs <pod-name> -n todoapp`
- Если сервис недоступен, проверьте endpoints: `kubectl get endpoints -n todoapp`
- Для проблем с DNS в busybox, убедитесь что используете полное DNS имя сервиса
