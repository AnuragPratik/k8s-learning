# Deploy a Stateful Application in a Kubernetes Cluster

## Deploying a Stateful Application in the Kubernetes Cluster

```cmd
kubectl create -f mysql-configmap.yaml
kubectl create -f mysql-services.yaml
kubectl create -f mysql-storageclass.yaml
kubectl create -f mysql-statefulset.yaml
kubectl get pods -l app=mysql --watch
kubectl describe pv | more
kubectl describe pvc | more
kubectl get statefulset
```

## Working with the Stateful Application

```cmd
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- mysql -h mysql-0.mysql -e "CREATE DATABASE mydb; CREATE TABLE mydb.notes (note VARCHAR(250)); INSERT INTO mydb.notes VALUES ('k8s Cloud Academy Lab');"
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- mysql -h mysql-read -e "SELECT * FROM mydb.notes"
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never -- bash -ic "while sleep 1; do mysql -h mysql-read -e 'SELECT @@server_id'; done"
kubectl get pod -o wide
kubectl drain <NODE_NAME_FOR_MYSQL_2_POD> --force --delete-local-data --ignore-daemonsets
kubectl get pod mysql-2 -o wide --watch
kubectl uncordon <NODE_NAME_FOR_MYSQL_2_POD>
kubectl delete pod mysql-2
kubectl get pod mysql-2 -o wide --watch
kubectl scale --replicas=5 statefulset mysql
kubectl get pods -l app=mysql --watch
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never -- bash -ic "while sleep 1; do mysql -h mysql-read -e 'SELECT @@server_id'; done"
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- mysql -h mysql-4.mysql -e "SELECT * FROM mydb.notes"
kubectl get services mysql-read
echo "  type: LoadBalancer" >> mysql-services.yaml
kubectl apply -f mysql-services.yaml
kubectl get services mysql-read
kubectl describe services mysql-read | grep "LoadBalancer Ingress"
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never -- bash -ic "while sleep 1; do mysql -h <EXTERNAL_LB_DNS> -e 'SELECT @@server_id'; done"
```