pipeline {
    agent {label 'build-agent'}
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        SONARQUBE_SERVER = 'SonarQube' // Name of your SonarQube server
        SONAR_AUTH_TOKEN = credentials('sonar-token') // Use Jenkins credentials for SonarQube token
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'       
               //sh 'mvn -T 2 clean install -DskipTests -Dcheckstyle.skip'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh """
                            mvn sonar:sonar \
                            -Dsonar.token=${SONAR_AUTH_TOKEN}
                        """
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                // Wait for SonarQube to analyze and check the quality gate status
                script {
                    timeout(time: 10, unit: 'MINUTES') { // Adjust timeout as needed
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
        stage('Create Docker Image') {
            steps {
                sh '''
                #!/bin/bash
                echo "-----PWD"
                pwd
                echo "-----ls"
                ls
                cat /etc/group
                echo "-----whoami"
                whoami
                echo "-----groups"
                groups
                echo 'Create Docker Image'
                #docker build -t rrddevops/spring-petclinic:${BUILD_NUMBER} -f scripts/docker/Dockerfile .
                #docker push rrddevops/spring-petclinic:${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
