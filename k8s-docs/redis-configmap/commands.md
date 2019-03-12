kubectl create configmap example-redis-config --from-file=redis-config
kubectl create -f https://k8s.io/examples/pods/config/redis-pod.yaml
kubectl exec -it redis redis-cli
    CONFIG GET maxmemory
    CONFIG GET maxmemory-policy