# INSTRUCTION.md

## 1. Deploy manifests
Усі маніфести знаходяться у папці `.infrastructure`.  
Щоб створити namespace, pod-и та сервіси — застосуйте:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/pod-a.yml
kubectl apply -f .infrastructure/pod-b.yml
kubectl apply -f .infrastructure/svc-clusterip.yml
kubectl apply -f .infrastructure/svc-nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
