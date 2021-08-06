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
        stage('Test') 
        {
            steps 
            {
                script{
                    def mvnHome = tool name: 'Maven3.6', type: 'maven'
                    sh "${mvnHome}/bin/mvn test"
                }
            }
            post 
            {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Sonar-Test')
        {
            steps
            {
                script
                {
                    def mvnHome = tool name: 'Maven3.6', type: 'maven'
                    withSonarQubeEnv('Sonar_server')
                    {
                        sh "${mvnHome}/bin/mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}

