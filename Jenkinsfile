pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/GithubMarbouh995/GestionBibliotheque.git'
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
                    withSonarQubeEnv('SonarQube') { // Using withSonarQubeEnv for better integration
                        sh 'mvn clean verify sonar:sonar'  // Combined build and analysis for efficiency
                     }
                }
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true // Ensure quality gate checks before proceeding
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Déploiement simulé réussi' // Replace with actual deployment steps
            }
        }
    }

    post {
        success {
            slackSend channel: '#your-slack-channel', color: 'good', message: "✅ ${env.JOB_NAME} - Build #${env.BUILD_NUMBER} successful! \nDetails: ${env.BUILD_URL}" // Enhanced Slack notification
        }
        failure {
            slackSend channel: '#your-slack-channel', color: 'danger', message: "❌ ${env.JOB_NAME} - Build #${env.BUILD_NUMBER} failed! \nDetails: ${env.BUILD_URL}" // Enhanced Slack notification
        }
        always {
          cleanWs()
        }

    }
}