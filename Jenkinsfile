node {      
  // Mark the code checkout 'stage'....        
    stage('Checkout the dockefile from GitHub') {            
      git branch: 'main', url: 'https://github.com/ooghenekaro/test1.git'
        
    }        
    // Build and Deploy to ACR 'stage'...        
    stage('Build the Image and Push to Azure Container Registry') {                
      app = docker.build('boboacr.azurecr.io/nodejswebapp')                
      withDockerRegistry([credentialsId: 'karo-acr', url: 'https://boboacr.azurecr.io']) {                
      app.push("${env.BUILD_NUMBER}")                
      app.push('latest')                
      }        
     }        
     stage('Build the Kubernetes YAML Files for New App') {
<The code here will differ depending on the YAMLs used for the application>        
  }        
  stage('Delpoying the App on Azure Kubernetes Service') {            
    app = docker.image('boboacr.azurecr.io/nodejswebapp:latest')            
    withDockerRegistry([credentialsId: 'boboacr', url: 'https://boboacr.azurecr.io']) {            
    app.pull()            
    sh "kubectl application -f ."            
    }       
   }    
}

