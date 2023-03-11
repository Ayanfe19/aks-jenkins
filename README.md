# DEPLOY SPRING-BOOT APP ON AZURE KUBERNETES SERVICE USING JENKINS

![Spring-boot App running on AKS](https://github.com/Ayanfe19/aks-jenkins/blob/main/images/Spring%20boot%20App.png)

This is my implementation of [Zudonu's Tutorial](https://medium.com/@osomudeyazudonu/how-to-deploy-spring-boot-app-into-aks-cluster-using-jenkins-pipeline-and-kubectl-cli-plugin-e59e53df34ea) as seen here. My implementation is a bit different, I will also some issues I faced, and their resolutions.

These are the pre-requisites, and I'll explain how to properly implement each one.

1. Azure Kubernetes Cluster
2. Configured Jenkins Instance
3. Azure Container Registry

### Preparing the requirements
