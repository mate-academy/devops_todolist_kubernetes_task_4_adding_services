# Перевірка застосунку ToDo через Kubernetes Services

## Підготовка

Потрібні запущений Kubernetes-кластер, налаштований `kubectl` і доступ кластера до образів із маніфестів. Усі команди виконуйте з кореня цього репозиторію, де розташовані `README.md`, `src` та `.infrastructure`.

Перевірте поточний контекст і вузли:

```bash
kubectl config current-context
kubectl get nodes -o wide
```

### Застосування маніфестів

Спочатку створіть namespace, потім решту ресурсів:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml \
  -f .infrastructure/ClusterIP.yml \
  -f .infrastructure/NodePort.yml \
  -f .infrastructure/busybox.yml

kubectl -n todoapp wait --for=condition=Ready pod/todoapp pod/todoapp-2 --timeout=180s
kubectl -n todoapp wait --for=condition=Ready pod/busybox --timeout=180s
kubectl -n todoapp get pods -o wide --show-labels
kubectl -n todoapp get services
```

Очікуйте готовність `1/1` для обох Pod застосунку. У вихідному коді цього репозиторію `/api/ready` повертає помилку протягом перших 30 секунд після запуску, тому Pod може не одразу стати готовим.

Очікувані Service:

| Назва | Тип | Порт Service | Порт застосунку | NodePort |
| --- | --- | --- | --- | --- |
| `todoapp-clusterip` | ClusterIP | 80 | 8080 | — |
| `todo-nodeport-app` | NodePort | 80 | 8080 | 30007 |

Перевірте, що кожен Service має обидва Pod серед своїх кінцевих точок:

```bash
kubectl -n todoapp get endpointslices \
  -l kubernetes.io/service-name=todoapp-clusterip -o yaml
kubectl -n todoapp get endpointslices \
  -l kubernetes.io/service-name=todo-nodeport-app -o yaml
```

Для кожного Service мають бути дві IP-адреси Pod із `conditions.ready: true` та порт `8080`. Адреси можна зіставити з виводом `kubectl get pods -o wide` вище.

## Перевірка ClusterIP за DNS-іменем із busybox

Перевірте DNS-ім'я Service з контейнера `busybox`:

```bash
kubectl -n todoapp exec busybox -- nslookup todoapp-clusterip.todoapp.svc.cluster.local
```

У відповіді має бути ClusterIP сервісу `todoapp-clusterip`. Суфікс `cluster.local` відповідає типовому домену кластера; якщо у вашому кластері він інший, використайте його або коротке ім'я `todoapp-clusterip` із Pod у тому самому namespace.

Виконайте HTTP-запити до застосунку через Service:

```bash
kubectl -n todoapp exec busybox -- curl -fsS -i \
  --connect-timeout 5 --max-time 10 \
  http://todoapp-clusterip.todoapp.svc.cluster.local/api/health

kubectl -n todoapp exec busybox -- curl -fsS -i \
  --connect-timeout 5 --max-time 10 \
  http://todoapp-clusterip.todoapp.svc.cluster.local/api/ready
```

Очікуйте HTTP `200` і тіла відповідей `Health OK` та `Readiness OK`. Із цього ж контейнера доступне коротке ім'я:

```bash
kubectl -n todoapp exec busybox -- curl -fsSL \
  --connect-timeout 5 --max-time 10 http://todoapp-clusterip/
```

Ця команда має повернути HTML сторінки застосунку, пройшовши HTTP-перенаправлення, якщо вони є. Внутрішнє DNS-ім'я Service призначене для звернень із кластера.

### Додаткова перевірка розподілу трафіку

Створіть кілька окремих HTTP-з'єднань із busybox. Параметр `check=clusterip` допоможе відрізнити ці запити від автоматичних probes у логах:

```bash
kubectl -n todoapp exec busybox -- sh -c '
  for i in 1 2 3 4 5 6 7 8 9 10; do
    curl -fsS --connect-timeout 5 --max-time 10 "http://todoapp-clusterip/api/health?check=clusterip"
    printf "\n"
  done
'

kubectl -n todoapp logs todoapp --since=2m --tail=100
kubectl -n todoapp logs todoapp-2 --since=2m --tail=100
```

Знайдіть запити з `check=clusterip` у логах обох Pod. Service автоматично розподіляє нові з'єднання між готовими Pod; суворе чергування або рівно 50/50 не гарантується. Якщо в невеликій вибірці запити потрапили лише в один Pod, перевірте кінцеві точки та повторіть серію.

## Перевірка через port-forward для Service

У першому терміналі запустіть:

```bash
kubectl -n todoapp port-forward service/todoapp-clusterip 8081:80
```

Залиште команду працювати. Локальний порт `8081` відповідає порту Service `80`, а його `targetPort` — порту Pod `8080`. Очікуваний вивід містить:

```text
Forwarding from 127.0.0.1:8081 -> 8080
```

У другому терміналі перевірте:

```bash
curl -fsS -i --connect-timeout 5 --max-time 10 http://127.0.0.1:8081/api/health
curl -fsS -i --connect-timeout 5 --max-time 10 http://127.0.0.1:8081/api/ready
```

Очікуйте HTTP `200` із відповідями `Health OK` і `Readiness OK`. Відкрийте сайт у браузері: <http://127.0.0.1:8081/>.

Зупиніть тунель за допомогою `Ctrl+C`. Якщо порт `8081` зайнятий, виберіть інший локальний порт і змініть URL відповідно.

`port-forward` вибирає один Pod за selector Service та направляє до нього весь трафік цього тунелю. Ця перевірка підтверджує доступність застосунку, але не демонструє балансування через ClusterIP.

## Доступ через NodePort

Перевірте, що Service має тип NodePort і порт `30007`:

```bash
kubectl -n todoapp get service todo-nodeport-app
kubectl -n todoapp describe service todo-nodeport-app
kubectl get nodes -o wide
```

У таблиці Service очікуйте `80:30007/TCP`, а в описі — `TargetPort: 8080/TCP`.

Звертайтеся до доступної з вашого комп'ютера IP-адреси вузла на порту `30007`. Наприклад, замініть `<NODE_IP>` адресою вашого вузла:

```bash
NODE_IP='<NODE_IP>'
curl -fsS -i --connect-timeout 5 --max-time 10 "http://${NODE_IP}:30007/api/health"
curl -fsS -i --connect-timeout 5 --max-time 10 "http://${NODE_IP}:30007/api/ready"
```

Очікуйте HTTP `200`. У браузері відкрийте `http://<NODE_IP>:30007/`. Для такого доступу має існувати мережевий маршрут до вузла, а порт `30007/TCP` має бути доступним із вашого комп'ютера.

### Docker Desktop

Для локального кластера Docker Desktop спробуйте адресу localhost:

```bash
curl -fsS -i --connect-timeout 5 --max-time 10 http://127.0.0.1:30007/api/health
```

Адреса сайту в браузері: <http://127.0.0.1:30007/>. Доступність NodePort через localhost залежить від налаштувань і способу створення кластера Docker Desktop. Якщо localhost недоступний, перевірте мережевий доступ до вузла та налаштування експонування портів вашого кластера. Перевірка через `port-forward` із попереднього розділу не підтверджує доступність NodePort.
