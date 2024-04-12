# Django ToDo list

This is a todo list web application with basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Explore

Try it out by installing the requirements (the following commands work only with Python 3.8 and higher, due to Django 4):

```
pip install -r requirements.txt
```

Create a database schema:

```
python manage.py migrate
```

And then start the server (default is http://localhost:8000):

```
python manage.py runserver
```

Now you can browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Create a kubernetes manifest for a pod which will containa ToDo app container:

1. Fork this repository.
1. Modify pod manifest to deploy a second same pod with a different name.
1. Add labels to pods “app: todolist”
1. Create a manifest for a ClusterIP service, which should balance traffic between two pods
1. Create a manifest for a NodePort service, which should expose an application on a Node Level
1. Set all env values for the container from pod’s manifest
1. `README.md` should contain instructions on how to test an app by calling a ClusterIP service DNS from a busybox container
1. `README.md` file should contain instructions on how to test ToDo application using the service `port-forward` command
1. `README.md` should contain instruction on how to access an app using a NodePort Service
1. Create PR with your changes and attach it for validation on a platform.

### Instruction

## Instructions on how to test ToDo application using cluster Ip service with port-forward command

1. start manifest:

 - kubectl apply -f clusterIp.yml

You should got such answer: 

service/todoapp-service created


2. Get name of service:

- kubectl get services -o wide

3. Check service:

- kubectl port-forward service/todoapp-service 8081:80 -n todoapp

4. Check on lokalhost:

- http://localhost:8081


## Test app with ClusterIP

kubectl -n {namespace} exec -it busybox -- sh


curl http://{service_name}.{namespace}.svc.cluster.local

Our case: => curl http://todoapp.todoapp.svc.cluster.local


## Instructions on how to test ToDo application using cluster Ip service with port-forward command

1. start manifest:

- kubectl apply -f nodeport.yml

You should got such answer: 

service/todoapp-nodeport-service created

2. Get name of service:

- kubectl get services -o wide

3. Check service:

Externall Access:

http://"nodeIP":30007
```
on localhost:

http://localhost:30007 


