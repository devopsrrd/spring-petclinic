pipeline {
    agent {label 'build-agent'}

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
                sh 'sudo docker build -t springboot:${BUILD_NUMBER} .'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}