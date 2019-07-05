# setup of kubectl, docker in WSL (Ubuntu) for windows 10

## setting up kubectl binary

```cmd
curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version
minikube ip
kubectl config set-cluster minikube --server=https://172.25.188.173:8443 --certificate-authority=/mnt/c/Users/Anurag/.minikube/ca.crt
kubectl config set-credentials minikube --client-certificate=/mnt/c/Users/Anurag/.minikube/client.crt --client-key=/mnt/c/Users/Anurag/.minikube/client.key
kubectl config set-context minikube --cluster=minikube --user=minikube
kubectl config use-context minikube
```

## setting up kubectl autocomplete and k alias

```cmd
source <(kubectl completion bash)
echo "if [ $commands[kubectl] ]; then source <(kubectl completion bash); fi" >> ~/.bashrc
echo "alias k=kubectl" >> ~/.bashrc
echo "complete -F __start_kubectl k" >> ~/.bashrc
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
echo "export DOCKER_HOST=tcp://172.25.188.173:2376" >> ~/.bashrc
echo "export DOCKER_CERT_PATH=/mnt/c/Users/Anurag/.minikube/certs" >> ~/.bashrc
source ~/.bashrc
```

## wsl on conemu, zsh setup

```cmd
conemu setup for wsl - <https://conemu.github.io/en/BashOnWindows.html>
zsh setup for wsl - <https://blog.joaograssi.com/windows-subsystem-for-linux-with-oh-my-zsh-conemu/

source <(kubectl completion zsh)  # setup autocomplete in zsh into the current shell
echo "if [ $commands[kubectl] ]; then source <(kubectl completion zsh); fi" >> ~/.zshrc # add autocomplete permanently to your zsh shell

echo "export DOCKER_TLS_VERIFY=1" >> ~/.zshrc
echo "export DOCKER_HOST=tcp://172.25.188.173:2376" >> ~/.zshrc
echo "export DOCKER_CERT_PATH=/mnt/c/Users/Anurag/.minikube/certs" >> ~/.zshrc
source ~/.zshrc
```
