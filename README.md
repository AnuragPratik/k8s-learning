# Kubernetes commands

## simple Commands

```cmd
kubectl get pods
kubectl create -f hello-world-pod.yml
kubectl describe pod hello-world
kubectl delete pod hello-world
kubectl create -f hello-world-service.yml
kubectl get services
kubectl describe service hello-world
kubectl get nodes
kubectl describe node docker-for-desktop
kubectl get namespaces
kubectl create -f k8s-ap-namespace.yml
kubectl create -f example-pod.yml --namespace k8s-ap
kubectl get pods --namespace k8s-ap
kubectl logs example -c redis --namespace k8s-ap
kubectl logs example -c server --namespace k8s-ap
kubectl logs example -c counter --namespace k8s-ap
kubectl logs example -c poller --namespace k8s-ap
kubectl delete pod hello-world
kubectl delete service hello-world
kubectl delete pod example --namespace k8s-ap
kubectl delete namespace k8s-ap
```

## multi-tier Commands

```cmd
kubectl create -f example-data-tier-service.yml
kubectl create -f example-data-tier-pod.yml
kubectl create -f example-app-tier-service.yml
kubectl create -f example-app-tier-pod.yml
kubectl create -f example-support-tier-pod.yml
kubectl logs example-data-tier -c redis
kubectl logs example-app-tier -c server
kubectl logs example-support-tier -c poller
kubectl logs example-support-tier -c counter
```

## deployments

```cmd
kubectl create -f example-data-tier-service.yml
kubectl create -f example-data-tier-pod.yml
kubectl create -f example-app-tier-service.yml
kubectl create -f example-app-tier-pod.yml
kubectl create -f example-support-tier-pod.yml
kubectl get deployments
kubectl scale deployment example-support-tier --replicas 10
kubectl get deployments
kubectl get pods
kubectl delete pod example-support-tier-8497ff4686-qmlvp
kubectl scale deployment example-app-tier --replicas 10
kubectl get deployments
kubectl get pods
```

## autoscaling

```cmd
kubectl top pods
kubectl top nodes
kubectl apply -f example-app-tier-deployment.yml
kubectl autoscale deployment example-app-tier --min 1 --max 10 --cpu-percent 70
kubectl edit hpa example-app-tier
kubectl delete hpa example-app-tier
kubectl edit deployment example-app-tier
kubectl rollout status deployment example-app-tier
```

## rollouts

```cmd
kubectl edit deployment example-app-tier
kubectl rollout status deployment example-app-tier
kubectl rollout pause deployment example-app-tier
kubectl rollout resume deployment example-app-tier
kubectl rollout undo deployment example-app-tier
kubectl rollout history deployment example-app-tier
kubectl rollout undo deployment example-app-tier --to-revision 3
```

## volumes

```cmd
kubectl exec example-data-tier-985f74597-rn2mr -it -- redis-cli set foo bar
kubectl exec example-data-tier-985f74597-rn2mr -it -- redis-cli get foo
```

## secret & config maps

```cmd
kubectl create secret generic api-keys --from-literal=key1=placeholder1 --from-literal=key2=placeholder2
kubectl create -f config-map.yml
kubectl apply -f example.yml
kubectl exec example -- env
kubectl delete pod example
```