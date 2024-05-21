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
---

CLone the repository and run the following command
1. kubectl apply -f  "all  .yml files from .infrastructure directory to" -n todoapp
2. kubectl get pods -n todoapp   #to check if  all pods are running correctly
3.kubectl describe svc todoapp-clusterip -n todoapp    #to observe the status of cluster ip 
5.  kubectl describe svc todoapp-nodeport -n todoapp  #to check if all node is running
4. kubectl exec -it busybox -n todoapp -- sh #to run tru shell of  busybox
5. kubectl exec -it busybox -n todoapp -- nslookup todoapp-clusterip
6.kubectl exec -it busybox -n todoapp -- sh -c "curl http://todoapp-clusterip:8080"
7. kubectl exec -it busybox -n todoapp -- sh -c "curl http://10.1.0.110:8080"
8.kubectl exec -it busybox -n todoapp -- sh -c "curl http://10.1.0.111:8080"
9.kubectl exec -it busybox -n todoapp -- sh -c "curl http://todoapp-clusterip"
10. kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
