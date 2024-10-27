pipeline {
    agent {label 'build-agent'}
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'       
               //sh 'mvn -T 2 clean install -DskipTests -Dcheckstyle.skip'
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
                docker build -t rrddevops/spring-petclinic:${BUILD_NUMBER} -f scripts/docker/Dockerfile .
                docker push rrddevops/spring-petclinic:${BUILD_NUMBER}
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
