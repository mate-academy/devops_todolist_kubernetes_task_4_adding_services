## Environment Variables

The application containers in `.infrastructure/todoapp-pod.yml` rely on the following environment variables passed directly via the pod manifests (`env` section):

| Variable Name | Description | Example / Default Value |
| :--- | :--- | :--- |
| `DATABASE_URL` | Database connection string | `sqlite:///./todoapp.db` |
| `SECRET_KEY` | Secret key used for session/application security | `your-secret-key-here` |
| `DEBUG` | Enables or disables debug mode | `True` |

## Testing ClusterIP Service DNS from a busybox Container

### 1. Execute Shell inside busybox Container
```bash
kubectl exec -it busybox -n todoapp -- sh
```

### 2. Test Endpoints using Service DNS
```bash
curl http://todoapp-service.todoapp.svc.cluster.local
```

## Testing the ToDo Application using kubectl port-forward on Service

### 1. Start Port Forwarding to ClusterIP Service

```bash
kubectl port-forward svc/todoapp-service 8080:80 -n todoapp
```

### 2. Test Endpoints Locally on Browser

http://localhost:8080/

## Accessing the Application using NodePort Service

### On browser

http://localhost:30007/

### On terminal

```bash
curl http://localhost:30007
```
