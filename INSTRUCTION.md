How to test an app by calling a ClusterIP service DNS from a busybox container?

Для цього спочатку потрібно все запустити, наступними командами:

спочатку!!:

kubectl apply -f .infrastructure/namespace.yml

Далі:

kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml

Після чого виконати наступну команду, за допомогою якої ми підключимось до busybox контейнера, та зробимо curl запит:
kubectl exec busybox -n todoapp -- sh

Далі потрібно написати:
curl http://todoapp-service.todoapp.svc.cluster.local


How to test ToDo application using the service port-forward command?

За допомогою команди:
kubectl port-forward service/todoapp-service <бажаний ваш порт>:80    (для прикладу знизу візьмем 8082:80)

Далі в браузері перейдіть по посиланню:
localhost:8082 

How to access an app using a NodePort Service
Спочатку треба поняти через який порт ми хочемо мати звязок з нодою, для цього в файлі .infrastructure/nodeport.yml треба ввести бажаний порт, в поле nodePort, перед тим як запускати команду:

kubectl apply -f .infrastructure/nodeport.yml

Далі в браузері треба зайти на <node-ip>:<порт який вводили в nodePort в файлы .infrastructure/nodeport.yml>