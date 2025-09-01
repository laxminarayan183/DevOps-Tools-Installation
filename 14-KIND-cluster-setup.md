1. install docker
```
sudo apt-get install docker.io
sudo usermod -aG docker $USER && newgrp docker
```

## Installing KIND cluster
2. create vim install_kind.sh paste below code in that and save

```
#!/bin/bash

# For AMD64 / x86_64

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo cp ./kind /usr/local/bin/kind
rm -rf kind
```

- give permission to that file to execute
```
chmod +x install_kind.sh
``` 
- now run 
```
./install_kind.sh
```
- to check
```
kind --version
```
3. Creating Cluster
-  create a config file - vim config.yml and paste it in below
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.30.0
- role: worker
  image: kindest/node:v1.30.0
- role: worker
  image: kindest/node:v1.30.0
```

- Create a 3-node Kubernetes cluster using Kind:
  ```bash
  kind create cluster --config=config.yml --name=my-cluster
  ```

- Check cluster information:
  ```bash
  kubectl cluster-info --context kind-kind
  kubectl get nodes
  kind get clusters
  ```




## Installing kubectl
4. create a vim install_kubectl file in tha paste below and save

```  
#!/bin/bash

# Variables
VERSION="v1.30.0"
URL="https://dl.k8s.io/release/${VERSION}/bin/linux/amd64/kubectl"
INSTALL_DIR="/usr/local/bin"

# Download and install kubectl
curl -LO "$URL"
chmod +x kubectl
sudo mv kubectl $INSTALL_DIR/
kubectl version --client

# Clean up
rm -f kubectl

echo "kubectl installation complete."

```

- give permission to that file to execute
```
chmod +x install_kubectl.sh
``` 
- now run 
```
./install_kubectl.sh
```
- to check
```
kubectl get nodes
```