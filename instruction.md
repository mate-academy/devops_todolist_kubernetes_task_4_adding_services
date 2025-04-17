# Updated Instructions

## Introduction

This guide assumes you are deploying the `todoapp` application on Kubernetes. Ensure that you have the proper Kubernetes
manifests configured.

---

## Services Overview

The `todoapp` application has the following services defined in the Kubernetes manifests:

1. **ClusterIP Service**
   - Name: `todoapp-clusterip-service`
   - Port: `80`

2. **NodePort Service**
   - Name: `todoapp-nodeport-service`
   - Port: `80`
   - NodePort: `30007`

---

## Steps to Deploy

### 1. Apply the Manifests

Run the following command to apply the Kubernetes manifests:

```bash
kubectl apply -f todoapp-deployment.yaml
kubectl apply -f todoapp-service.yaml
```

Ensure the correct manifests are in place for `todoapp-clusterip-service` and `todoapp-nodeport-service`.

---

### 2. Access the ClusterIP Service

The `todoapp-clusterip-service` is accessible within the cluster through port `80`.

Example command to test connectivity from within a pod in the cluster:

```bash
curl http://todoapp-clusterip-service:80
```

---

### 3. Access the NodePort Service

The `todoapp-nodeport-service` exposes the application to external clients through the node's IP on port `30007`.

Example to test:

```bash
curl http://<NODE_IP>:30007
```

Replace `<NODE_IP>` with the actual IP address of the node.

---

### 4. Verify Deployed Services

Run the following command to verify the services:

```bash
kubectl get services
```

You should see similar output:

```plaintext
NAME                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
todoapp-clusterip-service    ClusterIP   <CLUSTER_IP>    <none>        80/TCP          <AGE>
todoapp-nodeport-service     NodePort    <CLUSTER_IP>    <none>        80:30007/TCP    <AGE>
```

---

## Additional Notes

- Ensure your Kubernetes cluster is running and accessible.
- If you are running this using a cloud provider, confirm security group or firewall rules allow access through NodePort
  `30007`.