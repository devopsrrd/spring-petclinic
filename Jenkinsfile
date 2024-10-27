pipeline {
    agent { label 'ubuntu-agent' }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_HUB_REPO = 'rrddevops/spring-petclinic'
        SONARQUBE_SERVER = 'SonarQube' // Name of your SonarQube server
        SONAR_AUTH_TOKEN = credentials('sonar-token') // Use Jenkins credentials for SonarQube token
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn -T 4 clean install -DskipTests -Dcheckstyle.skip'
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
                script {
                    def customImage = docker.build("${DOCKER_HUB_REPO}:${BUILD_NUMBER}", "-f ./scripts/docker/Dockerfile .")
                    docker.withRegistry('', 'dockerhub-credentials') {
                        customImage.push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
