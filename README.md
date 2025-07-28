# Azure Voting App Deployment

This repository is part of a personal study on deploying infrastructure and applications on Microsoft Azure using Linux and CLI tools.

## 🧠 Purpose

The images in this repository represent key steps during the hands-on deployment of the Azure Voting App. They are intended for *educational use only* and document the process for *personal learning and future reference*.

## 📸 Screenshots

All screenshots are stored in the prints/ folder and named to follow the chronological order of the deployment process.

## ⚙️ How to Reproduce

The following steps were executed in an Ubuntu environment:

```bash
# Access Azure Cloud Shell or local terminal
az login

# Create resource group
az group create --name myResourceGroup --location eastus

# Create VM
az vm create --resource-group myResourceGroup \
  --name myVM --image UbuntuLTS --generate-ssh-keys

# Apply SSH key permission fix
chmod 400 myKey.pem

# Create Azure Container Registry
az acr create --resource-group myResourceGroup \
  --name myContainerRegistry --sku Basic

# Create AKS Cluster
az aks create --resource-group myResourceGroup \
  --name myAKSCluster --node-count 1 --generate-ssh-keys

# Connect to the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster

# Deploy the app
kubectl apply -f azure-vote-all-in-one-redis.yaml

# Create NSG rule for port 8080
az network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --name allow8080 \
  --protocol tcp --priority 1000 --destination-port-range 8080 --access allow --direction inbound
