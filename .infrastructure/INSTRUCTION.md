## Тестування ClusterIP з busybox:
```bash
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip.todoapp.svc.cluster.local
```

## Тестування через port-forward:
```bash
kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
```
Відкрийте http://localhost:8080

## Доступ через NodePort:
1. Знайдіть IP ноди:
```bash
kubectl get nodes -o wide
```
2. Відкрийте у браузері: `http://<NodeIP>:30080`
