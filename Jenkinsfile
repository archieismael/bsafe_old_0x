pipeline {     
    agent any     
        stages {         
		
			stage("Compile") {               
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

			stage ('Build Docker Image') {
					steps {
						sh """
						docker build -t bsafe_demo:001 .
						"""
					}
				}
				
			stage ('Push Docker Image') {
					steps {
							withCredentials([string(credentialsId: 'docker-id', variable: 'docker-id')]) {
								sh "docker login -u archieismael -p ${docker-id}"
							}						
								sh """
									docker push archieismael/bsafe_demo:001
								"""
						}

					}
				
		}
}

