pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    environment {
        registryName = 'spring-boot-app'
        registryCredential = 'acr-cred'
        registryUrl = 'ayanfeacr.azurecr.io'
        dockerImage = ''
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build registryName
                }
            }
        }

        stage('Upload Image to ACR') {
            steps {
                script {
                    docker.withRegistry("http://${registryUrl}", registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('K8S Deploy') {
            steps {
                script {
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                        sh('kubectl apply -f jenkins-aks-deploy-from-acr.yaml')
                    }
                }
            }
        }
    }
}
