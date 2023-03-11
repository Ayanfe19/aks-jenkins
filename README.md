# Deploy Spring Boot App on Azure Kubernetes Service Using Jenkins

![Spring-boot App running on AKS](https://github.com/Ayanfe19/aks-jenkins/blob/main/images/Spring%20boot%20App.png)


This is my implementation of [Zudonu's Tutorial](https://medium.com/@osomudeyazudonu/how-to-deploy-spring-boot-app-into-aks-cluster-using-jenkins-pipeline-and-kubectl-cli-plugin-e59e53df34ea) as seen here. My implementation is a bit different, I will also highlight some issues I faced, and their resolutions.

These are the pre-requisites, and I'll explain how to properly implement each one.

1. Configured Jenkins Instance
2. Azure Kubernetes Cluster
3. Azure Container Registry

## Preparing the requirements

### 1. Setting up Jenkins on an Azure VM.

My Jenkins Server was configured on an Azure VM.

For this, you will need an active Azure Subscription.

You can use this [Azure Documentation for the setup.](https://learn.microsoft.com/en-us/azure/developer/jenkins/configure-on-linux-vm)


### 2. Setting up the Kubernetes Cluster

Since we are working with Azure. We'll use AKS. 

You would want to ensure that you are using the same resource group for resource you intend to use on Azure.

It makes sense to use the same resource group you used for the VM hosting the Jenkins Instance.

You can use Azure Cloud Shell (on Azure Portal) or download Azure CLI for use on your own machine. I'll be working with Cloud Shell.

Note: You will need to have registered these Resource Providers as they are Container Operations on Azure.
i. Microsoft.Insights
ii. Microsoft.ContainerService

```
az provider register --namespace Microsoft.Insights
az provider register --namespace Microsoft.ContainerService
```

Some of them will be regisered for you when you run related commands.

Since we've covered all that, here is the command for creating your AKS Cluster.

```
az aks create -g aks-jenkins-rg -n aks-jenkins-cluster --enable-managed-identity --node-count 1 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys
```




