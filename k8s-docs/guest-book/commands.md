# Guest Book Application

## Setting Up

```
kubectl apply -f redis-master-deployment.yaml
kubectl get pods
kubectl logs -f POD-NAME
kubectl apply -f redis-master-service.yaml
kubectl get service
kubectl apply -f redis-slave-deployment.yaml
kubectl get pods
kubectl apply -f redis-slave-service.yaml
kubectl get services
kubectl apply -f frontend-deployment.yaml
kubectl get pods -l app=guestbook -l tier=frontend
kubectl apply -f frontend-service.yaml
kubectl get services
minikube service frontend --url
kubectl get service frontend
kubectl scale deployment frontend --replicas=5
kubectl get pods
kubectl scale deployment frontend --replicas=2
kubectl get pods
```

## Cleaning Up

```
kubectl delete deployment -l app=redis
kubectl delete service -l app=redis
kubectl delete deployment -l app=guestbook
kubectl delete service -l app=guestbook

kubectl get pods
```