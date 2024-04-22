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

In order to start application in the kubernetes you should execute the next steps:
1. Start Docker desktop application on your PC
2. Open a terminal and go to the directory '.infrastructure'
3. Create the namespace
kubectl apply -f namespace.yml
4. Create the pod with busybox
kubectl apply -f busybox.yml
5. Create pods with application
kubectl apply -f todoapp-pod.yml
6. Create clusterIp service
kubectl apply -f clusterIp.yml
7. Create Nodeport service
kubectl apply -f nodeport.yml
8. Check pods
kubectl get pods -n todoapp
9. In order to check the application in your browser you should execute the next command
kubectl port-forward pod/todoapp 8080:8080 -n todoapp
Now, the application is available for viewing in your browser by url
http://localhost:8080/
10. In order to check the application using the busybox and clusterIp service you should execute the next command
To connect to pod (enter inside)
kubectl -n todoapp exec -it busybox -- sh
Then execute the next command
curl http://todoapp-clusterip-service.todoapp.svc.cluster.local:8080
In terminal you have to see html-code the application`s start page
11. In order to check the application using the NodePort Service you should enter in your browser
http://localhost:30007



