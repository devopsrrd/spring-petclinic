pipeline {
    agent {label 'build-agent'}
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'       
                sh 'mvn -T 2 clean install -DskipTests'
            }
        }
        stage('Create Docker Image') {
            steps {
                echo 'Create Docker Image'
                sleep 300
                sh 'docker build -t rrddevops/spring-petclinic:${BUILD_NUMBER} -f scripts/docker/Dockerfile .'
                sh 'docker push rrddevops/spring-petclinic:${BUILD_NUMBER}'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
