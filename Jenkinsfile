node {
    def app
    

		// Mark the code checkout 'stage'....
		stage('Checkout from Bitbucket') {
			checkout scm
		}


		// Build and Deploy to ACR 'stage'... 
		stage('Build and Push to Azure Container Registry') {
			app = docker.build('turotaildemocr.azurecr.io/event-service')
			docker.withRegistry('https://turotaildemocr.azurecr.io', 'azure_acr_credential') {
				app.push("${env.BUILD_NUMBER}")
				app.push('latest')
			}
		}

		// Pull, Run, and Test on ACS 'stage'... 
		stage('ACS Docker Pull and Run') {
	   		app = docker.image('turotaildemocr.azurecr.io/event-service:latest')
	   		docker.withRegistry('https://turotaildemocr.azurecr.io', 'azure_acr_credential') {
				app.pull()
				//app.run('--name event-service -p 8082:8082')
                                sh '/usr/local/bin/docker-compose down'
                                sh '/usr/local/bin/docker-compose up -d'
			}
		}

   
}
