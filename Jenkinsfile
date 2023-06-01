node {      
  // Mark the code checkout 'stage'....        
    stage('Checkout the dockefile from GitHub') {            
      git branch: 'main', url: 'https://github.com/ooghenekaro/test1.git'
        
    }        
    
  stage('Build and Push to Azure Container Registry') {
			app = docker.build('boboacr.azurecr.io/nodejswebapp')
			docker.withRegistry('https://boboacr.azurecr.io', 'karo-acr') {
				app.push("${env.BUILD_NUMBER}")
				app.push('latest')
			}
		}
  stage('Delpoying the App on Azure Kubernetes Service') {            
    app = docker.image('boboacr.azurecr.io/nodejswebapp:latest')            
    withDockerRegistry([credentialsId: 'boboacr', url: 'https://boboacr.azurecr.io']) {            
    app.pull()            
    sh "kubectl application -f ."            
    }       
   }    
}

