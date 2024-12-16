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
                withCredentials([string(credentialsId: 'GestionBibliotheque', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            mvn sonar:sonar \
                            -Dsonar.projectKey=GestionBibliotheque-Jenkins \
                            -Dsonar.projectName='GestionBibliotheque-Jenkins' \
                            -Dsonar.host.url=\${SONAR_SERVER} \
                            -Dsonar.login=\${SONAR_TOKEN} \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                }
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Déploiement simulé réussi'
            }
        }
    }
    post {
        success {
            slackSend message: "Successful completion of ${env.JOB_NAME}"
        }
        failure {
            slackSend message: "Build failed for ${env.JOB_NAME}"
        }
    }
}