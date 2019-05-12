# Kubernetes Observability - Logging, Monitoring, and Debugging

## cat command for file write

```cmd
cat << 'EOF' > hello-world.sh
echo "hello world"
EOF
```

## Using Probes to Better Understand Pod Health

```cmd
kubectl create -f pod-readiness.yaml
kubectl explain pod.spec.containers.readinessProbe.httpGet
kubectl describe pod readiness-http
kubectl exec readiness-http -- pkill httpd
kubectl describe pod readiness-http
kubectl create -f pod-liveness.yaml
watch kubectl describe pod liveness-tcpSocket
kubectl delete -f pod-readiness.yaml
kubectl delete -f pod-liveness.yaml
```

## Understanding Container Logging in Kubernetes

```cmd
# Create namespace
kubectl create namespace logs

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=logs

kubectl create -f pod-logs.yaml
kubectl logs pod-logs server
kubectl logs --help
kubectl logs -f --tail=1 --timestamps pod-logs client
kubectl create -f pod-webserver.yaml
kubectl expose pod webserver-logs --type=LoadBalancer
watch kubectl get services
kubectl logs webserver-logs
kubectl exec webserver-logs -- tail -10 conf/httpd.conf
kubectl cp webserver-logs:conf/httpd.conf local-copy-of-httpd.conf
```

## Kubernetes Logging Using a Logging Agent and the Sidecar Pattern

```cmd
s3_bucket=$(aws s3api list-buckets --query "Buckets[].Name" --output table | grep logs | tr -d \|)
kubectl create -f fluentd-sidecar-config.yaml
kubectl create -f pod-counter.yaml
```

## Monitoring Kubernetes Applications

```cmd
kubectl get pods --all-namespaces
kubectl get events -n logs
kubectl top
wget -O /tmp/metrics-server.zip https://github.com/cloudacademy/metrics-server/archive/master.zip
sudo apt install unzip
unzip -q -d /tmp /tmp/metrics-server.zip
kubectl create -f /tmp/metrics-server-master/deploy/1.8+/
kubectl top node
kubectl top pods -n logs
kubectl top pod -n logs --containers
kubectl top pod -n logs --containers -l test
```