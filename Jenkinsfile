pipeline{
	agent any
    stages {
		stage('Build image')
		{
			steps
			{
				sh 'docker build -t django_server:v1 .'
				sh 'docker image ls'
			}
			post{
				success{
					slackSend(color:"#00FF00",message:"building image success")
				}
			}
		}
		stage('Push image')
		{
			environment{
				DOCKER_USER = credentials('docker_username')
				DOCKER_PASSWORD = credentials('docker_password')
			}
			steps
			{
				sh './upload_docker.sh'
		 	}
			post{
				success{
					slackSend(color:"#00FF00",message:"successfully pushing the image")
				}
			}
		}
		stage('Deploy')
		{
			environment{
				DOCKER_USER = credentials('docker_username')
				DOCKER_PASSWORD = credentials('docker_password')
			}
			steps
			{
				sh 'docker run -d -p 127.0.0.1:5000:8000 --name booster_project django_server:v1'
			}
			post{
				success{
					slackSend(color:"#00FF00",message:"successful deploy")
				}
			}
		}

}

    }