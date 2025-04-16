https://docs.ansible.com/automation-controller/4.4/html/userguide/overview.html

## docker
### Install docker
#### Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

#### Add the repository to Apt sources:
##### Ubuntu
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

##### Debian
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

#### Install Docker
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

docker version
docker --version

### clean up
sudo apt autoremove

### Add your user to the 'docker' group
sudo usermod -aG docker $USER && newgrp docker

## minikube
### Install kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release\
/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)\
/bin/linux/amd64/kubectl"

chmod +x ./kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

### Install minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
sudo mv minikube /usr/local/bin/
sudo apt-get install -y conntrack

### Start minikube
minikube start --driver=docker

### dashboard
minikube dashboard

###
kubectl apply -k .
kubectl get pods -n awx
kubectl config set-context --current --namespace=awx

kubectl get pods -l "app.kubernetes.io/managed-by=awx-operator"
kubectl get svc -l "app.kubernetes.io/managed-by=awx-operator"
kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager

### get ansible awx url
minikube service -n awx awx-demo-service --url
### get admin password
kubectl get secret awx-demo-admin-password -o jsonpath="{.data.password}" | base64 --decode ; echo


~~~bash
openssl rand -base64 32 > SECRET_KEY
~~~