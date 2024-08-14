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

# How to run:
1. create namespace `kubectl apply -f .infrastructure/namespace.yml`

2. start pods `kubectl apply -f .infrastructure/todoapp-pod.yml`

3. start busybox pod `kubectl apply -f .infrastructure/busybox.yml`

4. start cluster ip service `kubectl apply -f .infrastructure/clusterip.yml`

5. start node port service `kubectl apply -f .infrastructure/nodeport.yml`

6. enter busybox container `kubectl -n todoapp exec -it busybox -- sh`
to check connection to your cluster ip from busybox run `curl http://todoapp-clusterip.todoapp.svc.cluster.local`

7. to check connection using port forwarding run `kubectl port-forward service/todoapp-clusterip 8081:80 -n todoapp`
while port forwarding is running in background you can acces your webapp using the link http://localhost:8081/

8. to check acces from nodeport use link http://localhost:30007/

9. to do the cleanup run `kubectl delete namespace todoapp cleanup`
