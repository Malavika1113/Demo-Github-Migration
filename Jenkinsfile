pipeline {
    agent any
    stages {
        stage('Builds') 
        { 
            steps 
            {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}
