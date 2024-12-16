pipeline {
    agent any
    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
        SONAR_TOKEN = credentials('sonar-token') // Or use a string credential if preferred
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/GithubMarbouh995/GestionBibliotheque.git'
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
                script {
                    def mvnHome = tool 'Maven 3' // Replace 'Maven 3' with the name you gave in Global Tool Configuration
                    withSonarQubeEnv('SonarQube') {  // Replace 'SonarQube' with the name in your SonarQube server config
                        sh "${mvnHome}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=GestionBibliotheque -Dsonar.login=${env.SONAR_TOKEN}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deployment simulated' // Replace with your actual deployment commands
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            slackSend channel: '#dev', color: 'good', message: "Pipeline '${env.JOB_NAME}' (#${env.BUILD_NUMBER}) succeeded."
        }
        failure {
            slackSend channel: '#dev', color: 'danger', message: "Pipeline '${env.JOB_NAME}' (#${env.BUILD_NUMBER}) failed."
        }
    }
}