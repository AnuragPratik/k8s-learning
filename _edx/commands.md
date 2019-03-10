# Commands used in edX course

```
* kubectl get pods -L k8s-app,label2
* kubectl get pods -l k8s-app=webserver
* minikube service web-service
* kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
* kubectl get configmaps my-config -o yaml
* kubectl create secret generic my-password --from-literal=password=mysqlpassword
* kubectl get secret my-password
* kubectl describe secret my-password
* minikube addons enable ingress
```