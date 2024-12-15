pipeline {
    agent any
    environment {
        MAVEN_HOME = tool 'Maven'
        // Définir les variables SonarQube
        SONAR_CREDS = credentials('GestionBibliotheque') // ID de vos credentials Jenkins
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
            

            // steps {
            //     withSonarQubeEnv('SonarQube') {
            //         sh '${MAVEN_HOME}/bin/mvn sonar:sonar'
            //     }
            // }

             steps {
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
            mail to: 'abd.marbouh@gmail.com',
                subject: 'Build Success',
                body: 'Le build a été complété avec succès.'
        }
        failure {
            mail to: 'abd.marbouh@gmail.com',
                subject: 'Build Failed',
                body: 'Le build a échoué.'
        }
    }
}
