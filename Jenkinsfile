pipeline {
    agent any
    
    triggers {
        pollSCM('H/5 * * * *')  // Poll every 5 minutes
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }
        
        stage('Test') {
            steps {
                sh 'dotnet test --verbosity normal'
            }
        }
        
        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: '**/bin/Release/**/*.dll', fingerprint: true
                junit '**/TestResults/*.xml'
            }
        }
    }
    
    post {
        always {
            cleanWs()  // Clean workspace
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
