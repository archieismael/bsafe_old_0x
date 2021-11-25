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
						docker build -t archieismael/bsafe_demo:001 .
						"""
					}
				}
				
				
		}
}

