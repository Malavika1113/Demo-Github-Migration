pipeline {
    agent any
    stages {
        stage('Builds') 
        { 
            steps 
            {
                def mvnHome = tool name: 'Maven3.6', type: 'maven'
                sh "${mvnHome}/bin/mvn -B -DskipTests clean package"
            }
        }
    }
}
