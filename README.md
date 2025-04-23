# Install crossplane

**Install docker**

    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    sudo apt-get install -y docker-ce

    sudo usermod -aG docker ${USER}
    newgrp docker

**Install KIND (K8S)**

    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind
    
    kind version

**Create a yaml file to bring up cluster**

    cat <<EOF >kind-config.yaml
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    nodes:
      - role: control-plane
      - role: worker
    EOF

**Command to create KIND Cluster**

    kind create cluster --config kind-config.yaml


**Install kubectl**

    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    chmod +x kubectl
    sudo mv kubectl /usr/local/bin/
    kubectl version --client

**Helm install**

    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh

**AWS cli2 install**

    sudo apt install unzip
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install

**Install crossplane**

    helm install crossplane \
    crossplane-stable/crossplane \
    --namespace crossplane-system \
    --create-namespace

**Create a Kubernetes secret with the AWS credentials**

    kubectl create secret \
    generic aws-secret \
    -n crossplane-system \
    --from-file=creds=./aws-credentials.txt

**Create a ProviderConfig**

     kubectl apply -f providerconfig.yml







    

  
