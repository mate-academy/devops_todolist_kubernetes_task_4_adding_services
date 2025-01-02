## Інструкції для тестування додатку

### Тестування через ClusterIP сервіс:
1. Запустіть контейнер BusyBox:
   ```bash
   kubectl run -i --tty busybox --image=busybox --restart=Never --rm

2. Перевірте доступність сервісу:
   wget -O- todolist-clusterip-service
