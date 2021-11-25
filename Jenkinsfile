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

        }
}

