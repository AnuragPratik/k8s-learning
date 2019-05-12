# Kubernetes Pod Design for Application Developers

## cat command for file write

```cmd
cat << 'EOF' > hello-world.sh
echo "hello world"
EOF
```

## Reviewing Pod Definition Basics

```cmd
kubectl explain Pod.spec | more
kubectl explain <Resource_Kind>.<Path_To_Field>
kubectl explain Pod.spec.containers.image
kubectl create -f first-pod.yaml
kubectl get pod
kubectl get pod first-pod -o yaml | more
kubectl delete pod first-pod
```

## Working With Pod Labels, Selectors, and Annotations

```cmd
# Create namespace
kubectl create namespace labels

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=labels

# Create the Pods
kubectl create -f pod-labels.yaml

kubectl get pods -L color,tier
kubectl get pods -L color,tier -l color
kubectl get pods -L color,tier -l '!color'
kubectl get pods -L color,tier -l 'color=red'
kubectl get pods -L color,tier -l 'color=red,tier!=frontend'
kubectl get pods -L color,tier -l 'color in (blue,green)'
kubectl describe pod red-frontend | grep Annotations
kubectl annotate pod red-frontend Lab- &&
kubectl describe pod red-frontend | grep Annotations
kubectl annotate --help
kubectl label --help
kubectl delete -f pod-labels.yaml
```

## Managing Pods with Deployments

```cmd
# Create namespace
kubectl create namespace deployments

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=deployments

kubectl create deployment --image=httpd:2.4.38 web-server --dry-run -o yaml
kubectl create deployment --image=httpd:2.4.38 web-server
kubectl describe deployments web-server
kubectl get pods
kubectl scale deployment web-server --replicas=6
kubectl get pods
kubectl rollout history deployment web-server
kubectl edit deployment web-server --record

## add ports section below image line, set containerPort 80

kubectl rollout status deployment web-server
kubectl rollout history deployment web-server
kubectl set image deployment web-server httpd=httpd:2.4.38-alpine --record
kubectl describe deployments web-server
kubectl rollout undo deployment web-server
kubectl expose deployment web-server --type=LoadBalancer
watch kubectl get services
```

## Using Jobs to Manage Pods that Run to Completion

```cmd
# Create namespace
kubectl create namespace jobs

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=jobs

kubectl create job one-off --image=alpine -- sleep 30
kubectl get jobs one-off -o yaml | more
kubectl explain job.spec | more
kubectl create -f pod-fail.yaml
watch kubectl describe jobs pod-fail
kubectl get pods
kubectl create -f cronjob-example.yaml
watch kubectl describe cronjob cronjob-example
```