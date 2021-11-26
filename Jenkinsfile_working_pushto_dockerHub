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

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push archieismael/bsafe_demo:002'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
