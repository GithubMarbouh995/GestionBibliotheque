pipeline {
    agent any
    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/GithubMarbouh995/GestionBibliotheque.git' // Or use credentials if preferred 
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Quality Analysis') {
            steps {
                withCredentials([string(credentialsId: 'GestionBibliotheque-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') { 
                        sh 'mvn clean verify sonar:sonar -Dsonar.login=${SONAR_TOKEN}'
                    }
                }
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deployment simulated' // Replace with your deployment logic
            }
        }
    }
    post {
        always {
            cleanWs() // Now this should work
        }
        success {
            slackSend channel: '#dev', color: 'good', message: "Pipeline '${env.JOB_NAME}' (#${env.BUILD_NUMBER}) succeeded."
        }
        failure {
            slackSend channel: '#dev', color: 'danger', message: "Pipeline '${env.JOB_NAME}' (#${env.BUILD_NUMBER}) failed."
        }
    }
}