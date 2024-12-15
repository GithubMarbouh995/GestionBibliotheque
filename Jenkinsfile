pipeline {
    agent any
    environment {
        MAVEN_HOME = tool 'Maven'
        // Définir les variables SonarQube
        // Supprimer SONAR_CREDS du bloc environment
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/GithubMarbouh995/GestionBibliotheque.git'
            }
        }
        stage('Build') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn test'
            }
        }
        stage('Quality Analysis') {
            steps {
                withCredentials([string(credentialsId: 'GestionBibliotheque', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${MAVEN_HOME}/bin/mvn sonar:sonar \
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
