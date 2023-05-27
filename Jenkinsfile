pipeline {
    agent any 

     options {
        timeout(time: 10, unit: 'MINUTES')
     }
    environment {
    registryName = "boboacr"
    registyUrl = "boboacr.azurecr.io"
    APP_NAME = "nodejswebapp"
    IMAGE_TAG = "latest"
  
    }
    stages { 
        stage('SCM Checkout') {
            steps{
           git branch: 'main', url: 'https://github.com/ooghenekaro/test1.git'
            }
        }
        // run sonarqube test
        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'ibt-sonarqube';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'ibt-sonar', installationName: 'IBT sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
              }
            }
        }
      stage('Build and Push Docker Image') {
      steps {
        // Build Docker image and push to your Docker registry
        script {
          def dockerImage = docker.build("${registryName}/${APP_NAME}:${env.IMAGE_TAG}")
          docker.withRegistry("${registryName}") {
            dockerImage.push()
          }
        }
      }
    }
      
   withKubeConfig(caCertificate: '', clusterName: 'boboCluster', contextName: '', credentialsId: 'karo-kubecong', namespace: '',     restrictKubeConfigAccess: false, serverUrl: '') {
           sh "kubectl apply -f deployment.yml"
    }  
    post { 
        always { 
           //slackSend message: 'Pipeline completed - Build deployed successfully '
           slackSend color: "good", message: "Build Successful, Image pushed to ACR and Application Deployed to AKS Cluster"
           }
    }
}
