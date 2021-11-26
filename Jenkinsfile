pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhubac')
	}

	stages {

		stage("Maven Project Build") {               
				steps {                    
					sh "mvn clean install"               
				}          
			}          

		stage('Project Unit Test') {
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
				sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
				sh 'docker tag $JOB_NAME:v1.$BUILD_ID archieismael/$JOB_NAME:v1.$BUILD_ID'
				sh 'docker tag $JOB_NAME:v1.$BUILD_ID archieismael/$JOB_NAME:latest'
			}
		}

		stage('Push Docker Build To Repo') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				
				sh 'echo JOBNAME:v1.BUILD_ID: $JOB_NAME:v1.$BUILD_ID'
				sh 'docker push archieismael/$JOB_NAME:v1.$BUILD_ID'
				sh 'docker push archieismael/$JOB_NAME:latest'
			}
		}

		stage('Pull Docker Build From Repo') {
			steps {
				sh 'docker run -p 8082:8080 -d --name vsafe archieismael/$JOB_NAME:latest'
			}
		}

		stage('Test Run Application from Docker Container') {
			steps {
				sh 'sleep 10'	
		                sh 'curl -i http://10.0.0.50:8082/bsafe/'
			}
		}

		stage('Removing Docker Container') {
			steps {
				sh "echo 'Removing used docker container'"
				sh 'sleep 10'
				sh 'docker container rm $(docker container ls -aq) --force'
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
