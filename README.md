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


## Testing the Application Using ClusterIP Service DNS from a Busybox Container

To verify that the `ClusterIP` service is correctly routing traffic to the `todoapp` pods, you can use a `busybox` container to test DNS resolution and connectivity.

### Steps:

1. **Deploy a Busybox Pod:**
   First, deploy a `busybox` pod in the same `todoapp` namespace that will be used to test DNS resolution.

   ```bash
   kubectl run busybox --image=busybox --restart=Never --namespace=todoapp -- sleep 3600
   ```
This command will create a busybox pod that will run for 1 hour (3600 seconds), allowing to execute commands within it.

2. **Execute DNS Resolution Test:** Once the busybox pod is running, you can use it to test the DNS resolution of the ClusterIP service:
    ```
    kubectl exec -n todoapp busybox -- nslookup todoapp-clusterip
    ```
    You should see an output that resolves the DNS name todoapp-clusterip to the corresponding ClusterIP address.

3. **Test Connectivity to the ToDo Application:**
After confirming DNS resolution, test the connectivity to the ToDo application using the wget command:
    ```
    kubectl exec -n todoapp busybox -- wget -O- todoapp-clusterip
    ```
    This command will attempt to access the ToDo application through the ClusterIP service, and you should see the HTML output of the application's homepage if everything is configured correctly.
4. **Cleanup:** Once you've finished testing, you can delete the busybox pod:
    ```
    kubectl delete pod busybox -n todoapp
    ```
