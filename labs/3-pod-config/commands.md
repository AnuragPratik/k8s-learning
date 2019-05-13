# Mastering Kubernetes Pod Configuration

## Requesting and Limiting Resources for Pods

```cmd
# Get the IP address of the Kubernetes master using the AWS CLI
master_ip=$(aws ec2 describe-instances \
  --region=us-west-2 \
  --filters "Name=tag:Name,Values=k8s-master" \
  --query "Reservations[0].Instances[0].PrivateIpAddress" \
  --output text)

# Securely copy the kubeonfig file
scp -o "ForwardAgent yes" ubuntu@$master_ip:~/kubeconfig ~/.kube/config

# Load completions
source <(kubectl completion bash)

# Download and install Metrics Server
wget -qO /tmp/metrics-server.zip https://github.com/cloudacademy/metrics-server/archive/master.zip
sudo apt install unzip && \
  unzip -q -d /tmp /tmp/metrics-server.zip && \
  kubectl create -f /tmp/metrics-server-master/deploy/1.8+/

kubectl create -f load.yaml
kubectl top --help
kubectl top pods
kubectl top nodes
kubectl create -f load-limited.yaml
kubectl top pods
kubectl delete -f load.yaml
kubectl delete -f load-limited.yaml
```

## Configuring Pod Security Contexts

```cmd
kubectl explain pod.spec.securityContext
kubectl explain pod.spec.containers.securityContext
kubectl create -f pod-no-security-context.yaml
kubectl exec security-context-test-1 -it -- ls /dev
kubectl delete -f pod-no-security-context.yaml
kubectl create -f pod-privileged.yaml
kubectl exec security-context-test-2 -it -- ls /dev
kubectl delete -f pod-privileged.yaml
kubectl create -f pod-runas.yaml
kubectl exec security-context-test-3 -it -- /bin/sh
ps
touch /tmp/test-file
exit
kubectl delete -f pod-runas.yaml
```

## Using Persistent Data with Pods

```cmd
# Create namespace
kubectl create namespace persistence

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=persistence

kubectl create -f pvc.yaml
kubectl get pvc
kubectl create -f db.yaml
kubectl exec db -it -- mongo testdb --quiet --eval 'db.messages.insert({"message": "I was here"}); db.messages.findOne().message'
kubectl delete -f db.yaml
kubectl create -f db.yam
kubectl exec db -it -- mongo testdb --quiet --eval 'db.messages.findOne().message'
```

## Configuring Pods using Data Stored in ConfigMaps

```cmd
# Create namespace
kubectl create namespace configmaps

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=configmaps

kubectl create configmap app-config --from-literal=DB_NAME=testdb --from-literal=COLLECTION_NAME=messages
kubectl get configmaps app-config -o yaml

kubectl create -f pod-configmap.yaml
kubectl exec db -it -- ls /config
kubectl exec db -it -- cat /config/DB_NAME && echo
kubectl create configmap --help
```

## StorIng and Accessing Sensitive Information Using Kubernetes Secrets

```cmd
# Create namespace
kubectl create namespace secrets

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=secrets

kubectl create secret generic app-secret --from-literal=password=123457
kubectl get secret app-secret -o yaml
kubectl get secret app-secret -o jsonpath="{.data.password}" | base64 --decode && echo
kubectl create -f pod-secret.yaml
kubectl exec pod-secret -- /bin/sh -c 'echo $PASSWORD'
```

## Using Kubernetes ServiceAccounts

```cmd
# Create namespace
kubectl create namespace serviceaccounts

# Set namespace as the default for the current context
kubectl config set-context $(kubectl config current-context) --namespace=serviceaccounts

kubectl run default-pod --generator=run-pod/v1 --image=mongo:4.0.6
kubectl get pod default-pod -o yaml | more
kubectl create serviceaccount app-sa
kubectl create -f pod-custom-sa.yaml
kubectl run default-pod --generator=run-pod/v1 --image=mongo:4.0.6 --serviceaccount=app-sa --dry-run -o yaml
kubectl get pod custom-sa-pod -o yaml | more
```