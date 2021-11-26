pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhubac')
	}

	stages {

		stage("Build") {               
				steps {                    
					sh "mvn clean install"               
				}          
			}          

		stage('Unit Test') {
				steps {
					sh 'mvn test'
				}
				post {
					always {
						junit 'target/surefire-reports/*.xml'
					}
				}
			}

		stage('Docker Build') {

			steps {
				sh 'docker build -t archieismael/bsafe_demo:002 .'
			}
		}

		stage('Docker Login/Push') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

				sh 'docker push archieismael/bsafe_demo:002'
			}
		}

		stage('Docker Run') {
			steps {
				sh 'docker run -p 8082:8080 -d --name vsafe archieismael/bsafe_demo:002'
			}
		}

		stage('Test Run Application from Docker Container') {
			steps {
				sh 'sleep 10'	
		                sh 'curl -i http://10.0.0.50:8082/bsafe/'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		    	sh "echo 'Removing used build'"
		    	sh 'docker image prune -a -f'
		}
	}

}
