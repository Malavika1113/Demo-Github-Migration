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
            steps 
            {
                script
                {
                   timeout(time: 1, unit: 'HOURS') 
                   {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') 
                        {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                   }
                }
            }
        }
        stage("Nexus Upload"){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'my-app', 
                        classifier: '', 
                        file: 'target/my-app-1.0.0.jar',
                        type: 'jar'
                    ]
                ], 
                    credentialsId: 'nexus-ids', 
                    groupId: 'com.mycompany.app', 
                    nexusUrl: '10.128.15.210:8085', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'simpleapp-release', 
                    version: '1.0.0'
            }
        }
    }
}

