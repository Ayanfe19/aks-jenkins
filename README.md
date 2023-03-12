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

Once you are done with the setup of the VM above. You need to ensure that Docker is available on this VM, and also that Jenkins have the proper access to use Docker.

Connect to the Jenkins VM and use the script docker-setup.sh for this setup.

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
sudo apt install docker-ce
```

Once Docker is installed. You want to ensure that Jenkins has permission to use docker

```
sudo groupadd docker
sudo usermod -aG docker $USER
```

You can switch to the Jenkins user to be sure that Jenkins can now run docker commands.

If your setup is for running Jenkins on a Docker Conatiner, simply ensure that your Dockerfile for the setup includes a script to install docker. See this [example](https://github.com/shazChaudhry/docker-jenkins/blob/ee0f386fd1706829b956cb2e723c0f2935496933/Dockerfile)


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

You will need to use Kubectl to interact with your cluster. To configure Kubectl to connect to your cluster use the command below

```
az aks get-credentials --resource-group aks-jenkins-rg --name aks-jenkins-cluster
```

Then verify that you can access the cluster using:
```
kubectl get nodes
```

This should show exacly 1 node, since our cluster was configured to have just 1 node.


### 3. Setting up the Azure Container Registry

```
az acr create — resource-group aks-jenkins-rg — name ayanfeacr — sku Standard — location eastus
```

You will need to integrate Azure Container Registry with Azure Kubernetes Service. The integration procedure will assign the AcrPull role to the Managed Identity of the Cluster.

This is done using the command here:

```
az aks update -n aks-jenkins-cluster -g aks-jenkins-rg --attach-acr ayanfeacr 
```


Now that we are done with the pre-requisites let's setup the Jenkins Pipeline.


### Step 1
### Set up the Credentials
There are 3 Crednetials we will be setting up for this Pipeline:
1. GitHub Credentials
2. ACR Credentials
3. Kubernetes Config Credentitals

To add Credentials to Jenkins, from the Dashboard **Manage Jenkins** >> **Manage Credentials** 

On the Global Scope, add a credential.

![Add Credentials in Jenkins]()

For GitHub Credentials and ACR Credentials the process is similar.

You can get your ACR Credentials from the 

![Where to get your ACR Credentials]()

For the Kubernetes Config Credentials, you can get the credentials from the CLI.


```
cat .kube/config > cluster-config
```

The command above sends the config credentials to a file, download it or copy the content to be used in your Jenkins Credentials

Use the Secret File type of credential.

![Set up K8S Config Credentials]()
 
### Step 2
### Create the Jenkins Pipeline

### Step 3
### Prepare the Kubernetes Manifest file


### Step 4
### Build the Pipeline


### Step 5
### Verify the Deployment


