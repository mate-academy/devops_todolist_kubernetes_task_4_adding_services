# Інструкція з тестування додатку

## **Тестування через ClusterIP (DNS)**
1. **Створіть BusyBox Pod:**
kubectl run busybox --image=busybox --rm -it --restart=Never

2. **Перевірте доступ до ClusterIP:**
wget http://todo-clusterip.default.svc.cluster.local


## **Тестування через port-forward**
1. **Запустіть port-forward:**
kubectl port-forward svc/todo-clusterip 8080:80 &

text
2. **Перейдіть за адресою:**
curl http://localhost:8080

## **Доступ через NodePort**
1. **Знайдіть IP вузла:**
kubectl get nodes -o jsonpath='{.items.status.addresses.address}'

text
2. **Перейдіть за адресою:**
curl http://<NODE_IP>:30020
