# setup of kubectl, docker in WSL (Ubuntu) for windows 10

## setting up kubectl binary

```cmd
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version
kubectl config set-cluster minikube --server=https://172.17.235.26:8443 --certificate-authority=/mnt/c/Users/Anurag/.minikube/ca.crt
kubectl config set-credentials minikube --client-certificate=/mnt/c/Users/Anurag/.minikube/client.crt --client-key=/mnt/c/Users/Anurag/.minikube/client.key
kubectl config set-context minikube --cluster=minikube --user=minikube
kubectl config use-context minikube
```

## setting up kubectl autocomplete

```cmd
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

## setting up docker

```cmd
sudo apt-get update -y
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update -y
sudo apt-get install -y docker-ce
sudo usermod -aG docker $USER
echo "export DOCKER_TLS_VERIFY=1" >> ~/.bashrc
echo "export DOCKER_HOST=tcp://172.17.235.26:2376" >> ~/.bashrc
echo "export DOCKER_CERT_PATH=/mnt/c/Users/Anurag/.minikube/certs" >> ~/.bashrc
echo "export DOCKER_API_VERSION=1.35" >> ~/.bashrc
source ~/.bashrc
```