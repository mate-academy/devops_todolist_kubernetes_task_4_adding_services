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
---
---
## Solution

I modified `busybox.yml` to use curleimages since busybox is deprecated

I modified `todoapp-pod.yml`:

1. added numbering to pod name
2. added labels metadata
3. modified probes timings
4. created second pod

---

I created Cluster IP service

* mentioning label from todoapp-pod in Selector spec

I applied namespace, todoapp-pod, clusteripsvc, busybox manifests

Cluster IP service test:

1. I used `kubectl exec -it -n todoapp curlpod -- sh` command to use curlimages pod's shell

    run `curl http://clusteripsvc.todoapp.svc.cluster.local/api/health`
    run `curl http://clusteripsvc.todoapp.svc.cluster.local/api/ready`

    commands with good results
2. I used port forward command `kubectl port-forward svc/clusteripsvc 8081:80 -n todoapp`

    addressed `127.0.0.1:8081/api/health` & `127.0.0.1:8081/api/ready` with good results
3. checked both pods logs to make sure the traffic is equally shared between 2 pods with

    `kubectl logs todoapppod1 -n todoapp` & `kubectl logs todoapppod2 -n todoapp`

    and got similar results on both pods:

    ```python
    .....
    [03/Jun/2024 19:03:44] "GET /api/health HTTP/1.1" 200 9
    [03/Jun/2024 19:03:49] "GET /api/ready HTTP/1.1" 200 12
    [03/Jun/2024 19:03:49] "GET /api/health HTTP/1.1" 200 9
    [03/Jun/2024 19:03:53] "GET /api/health HTTP/1.1" 200 9
    [03/Jun/2024 19:03:58] "GET /api/ready HTTP/1.1" 200 12
    [03/Jun/2024 19:03:58] "GET /api/health HTTP/1.1" 200 9
    ```

---

I created NodePort Service manifest which is exact replica of ClusterIP manifest with ammended type and added NodePort port:

* I used first  port among possible range 30001

I applied manifest

NodePort service test:

addressed `127.0.0.1:30001/api/health` & `127.0.0.1:30001/api/ready` with good results
