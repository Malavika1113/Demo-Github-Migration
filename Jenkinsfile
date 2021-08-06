pipeline {
    agent any
    stages {
        stage('Builds') 
        { 
            steps 
            {
                script{
                    def mvnHome = tool name: 'Maven3.6', type: 'maven'
                    sh "${mvnHome}/bin/mvn -B -DskipTests clean package"
                }
            }
        }
        stage('Test') {
            steps {
                scripts{
                    def mvnHome = tool name: 'Maven3.6', type: 'maven'
                    sh "${mvnHome}/bin/mvn test"
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
