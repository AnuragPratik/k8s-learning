# Minikube setup

* rename kubectl in docker directory to kubectl.exe.bak (C:\Program Files\Docker\Docker\resources\bin\kubectl.exe)
* remove kubernetes cluster from docker for windows setting
* exit docker
* kubectl - <https://storage.googleapis.com/kubernetes-release/release/v1.14.1/bin/windows/amd64/kubectl.exe> - <https://storage.googleapis.com/kubernetes-release/release/stable.txt>
* minikube - <https://storage.googleapis.com/minikube/releases/v1.0.1/minikube-windows-amd64.exe> - <https://github.com/kubernetes/minikube/releases>

```cmd
* set environment variable MINIKUBE_HOME=C:\Users\Anurag\
* minikube config set memory 4096
* minikube config set cpus 2
* minikube start --vm-driver hyperv --hyperv-virtual-switch "Default Switch"
* kubectl cluster-info
* kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080
* kubectl expose deployment hello-minikube --type=NodePort
* minikube service hello-minikube --url
* kubectl delete services hello-minikube
* kubectl delete deployment hello-minikube
* minikube stop (if doesn't work try minikube ssh && sudo poweroff)
* minikube docker-env
* @FOR /f "tokens=*" %i IN ('minikube docker-env') DO @%i
* docker ps
```

## docker for widows with kubernetes, setting up dashboard

<https://github.com/kubernetes/dashboard>
<https://github.com/kubernetes-retired/heapster/>

```cmd
* kubectl apply -f dashboard-adminuser.yaml
* kubectl apply -f dashboard-adminuser-role.yaml
* kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
* kubectl create -f https://raw.githubusercontent.com/kubernetes-retired/heapster/master/deploy/kube-config/influxdb/influxdb.yaml
* kubectl create -f https://raw.githubusercontent.com/kubernetes-retired/heapster/master/deploy/kube-config/influxdb/heapster.yaml
* kubectl create -f https://raw.githubusercontent.com/kubernetes-retired/heapster/master/deploy/kube-config/influxdb/grafana.yaml
* kubectl proxy
* http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
* kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
```
