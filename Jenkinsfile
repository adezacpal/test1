  pipeline {
     agent any
     
        environment {
        //once you create ACR in Azure cloud, use that here
        registryName = "boboacr/nodejswebapp"
        //- update your credentials ID after creating credentials for connecting to ACR
        registryCredential = 'karo-acr'
        dockerImage = ''
        registryUrl = 'boboacr.azurecr.io'
    }
       
        stage ('Build Docker image') {
            steps {
                
                script {
                    dockerImage = docker.build registryName
                }
            }
        }
       
    // Uploading Docker images into ACR
    stage('Upload Image to ACR') {
     steps{   
         script {
            docker.withRegistry( "http://${registryUrl}", registryCredential ) {
            dockerImage.push()
            
        }
      }
    } 
  }
}

