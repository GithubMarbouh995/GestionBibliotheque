pipeline {
    agent any
    
    tools {
        maven 'Maven'
        jdk 'JDK17'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/GithubMarbouh995/GestionBibliotheque.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile -Dmaven.compiler.source=17 -Dmaven.compiler.target=17'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -Dmaven.compiler.source=17 -Dmaven.compiler.target=17'
            }
        }
        stage('Quality Analysis') {
            steps {
                withCredentials([string(credentialsId: 'GestionBibliotheque', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            mvn sonar:sonar \
                            -Dmaven.compiler.source=17 \
                            -Dmaven.compiler.target=17 \
                            -Dsonar.java.source=17 \
                            -Dsonar.host.url=${SONAR_SERVER} \
                            -Dsonar.login=${SONAR_TOKEN} \
                            -Dsonar.projectKey=GestionBibliotheque-Jenkins \
                            -Dsonar.projectName='GestionBibliotheque-Jenkins' \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                }
                // Attendre le résultat de la Quality Gate
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
