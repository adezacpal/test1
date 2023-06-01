pipeline {
    agent any 

     options {
        timeout(time: 10, unit: 'MINUTES')
     }
    environment {
    ACR_NAME = "boboacr"
    registyUrl = "boboacr.azurecr.io"
    IMAGE_NAME = "nodejswebapp"
    IMAGE_TAG = "latest"
   registryCredential = "karo-acr"
  
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
    
    stage ('Build Docker image') {
        steps {
                script {
                    //dockerImage = docker.build registryUrl
                 def dockerImage = docker.build("${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${IMAGE_TAG}", '.') 
                }
            }
       }
        
    // Uploading Docker images into ACR
     //   stage('Upload Image to ACR') {
       //  steps{   
         //    script {
        //     docker.withRegistry( "http://boboacr.azurecr.io", 'registryCredential' ) {
             //   dockerImage.push()
               // }
            //}
          //}
        //}
        
     stage('Build and Push Docker Image') {
      steps {
        script {
            docker.withRegistry("https://${ACR_NAME}.azurecr.io", 'registryCredential') {
            def dockerImage = docker.build("${ACR_NAME}.azurecr.io/${IMAGE_NAME}:${IMAGE_TAG}", '.')
             dockerImage.push()
          }
        }
      }
    }
    stage ('K8S Deploy') {
          steps {
            script {
   withKubeConfig(caCertificate: '', clusterName: 'boboCluster', contextName: '', credentialsId: 'karo-kubecong', namespace: '',     restrictKubeConfigAccess: false, serverUrl: '') {
           sh "kubectl apply -f deployment.yml"
               } 
            }
          }
      }
    }
   // post { 
     //   always { 
           //slackSend message: 'Pipeline completed - Build deployed successfully '
          // slackSend color: "good", message: "Build Successful, Image pushed to ACR and Application Deployed to AKS Cluster"
    //}
  //}
 }
