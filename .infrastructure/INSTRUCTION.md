# ToDo Application — Інструкція з тестування

## 1. Тестування через ClusterIP DNS з busybox

Запусти тимчасовий busybox контейнер всередині кластера:

```sh
kubectl run busybox --image=busybox:1.28 --rm -it --restart=Never -n todoapp -- /bin/sh
```

Всередині контейнера виконай запит до ClusterIP сервісу через DNS:

```sh
wget -qO- http://todoapp-clusterip.todoapp.svc.cluster.local/api/health/live
```

DNS формат: `<service-name>.<namespace>.svc.cluster.local`

---

## 2. Тестування через port-forward

Прокинь порт з сервісу на локальну машину:

```sh
kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
```

Відкрий у браузері або виконай у терміналі:

```sh
curl http://localhost:8080/api/health/live
```

---

## 3. Доступ до застосунку через NodePort

Отримай IP ноди:

```sh
kubectl get nodes -o wide
```

Відкрий у браузері або виконай:

```sh
curl http://<NODE_IP>:30080/api/health/live
```

Де `<NODE_IP>` — зовнішній IP ноди з попередньої команди, `30080` — nodePort зі маніфесту.

Для minikube використовуй:

```sh
minikube service todoapp-nodeport -n todoapp --url
```