pipeline {
    agent {
        kubernetes{
            label 'build-agent' // Label for the pod template
            defaultContainer 'docker' // Default container for pipeline steps
            yaml
'''
apiVersion: "v1"
kind: "Pod"
metadata:
  annotations:
    kubernetes.jenkins.io/last-refresh: "1730017833565"
  labels:
    jenkins: "slave"
    jenkins/label-digest: "793cb25eb1c83fd9826163adaa48d82fa549b60e"
    jenkins/label: "build-agent"
    kubernetes.jenkins.io/controller: "tp___jenkins-service_devops-tools_svc_cluster_local_8080x"
  name: "build-agent-25fft"
  namespace: "devops-tools"
spec:
  containers:
  - command:
    - "/jenkins-agent/jenkins-agent"
  volumes:
  - hostPath:
      path: "/var/run/docker.sock"
    name: "volume-0"
  - emptyDir:
      medium: ""
    name: "workspace-volume"
  - emptyDir: {}
    name: "jenkins-agent"
    env:
    - name: "JENKINS_SECRET"
      value: "********"
    - name: "JENKINS_TUNNEL"
      value: "jenkins-service:50000"
    - name: "JENKINS_AGENT_NAME"
      value: "build-agent-25fft"
    - name: "JENKINS_AGENT_FILE"
      value: "/jenkins-agent/agent.jar"
    - name: "REMOTING_OPTS"
      value: "-noReconnectAfter 1d"
    - name: "JENKINS_NAME"
      value: "build-agent-25fft"
    - name: "JENKINS_AGENT_WORKDIR"
      value: "/home/jenkins/agent"
    - name: "JENKINS_URL"
      value: "http://jenkins-service.devops-tools.svc.cluster.local:8080/"
    image: "rrddevops/build-agent:v1.0.1"
    imagePullPolicy: "Always"
    name: "build-agent"
    resources: {}
    securityContext:
      privileged: true
    tty: false
    volumeMounts:
    - mountPath: "/var/run/docker.sock"
      name: "volume-0"
      readOnly: false
    - mountPath: "/home/jenkins/agent"
      name: "workspace-volume"
      readOnly: false
    - mountPath: "/jenkins-agent"
      name: "jenkins-agent"
      readOnly: true
    workingDir: "/home/jenkins/agent"
  hostNetwork: false
  initContainers:
  - command:
    - "/bin/sh"
    - "-c"
    - "cp $(command -v jenkins-agent) /jenkins-agent/jenkins-agent;cp -R /usr/share/jenkins/. /jenkins-agent"
    image: "jenkins/inbound-agent:3261.v9c670a_4748a_9-2"
    name: "set-up-jenkins-agent"
    volumeMounts:
    - mountPath: "/jenkins-agent"
      name: "jenkins-agent"
  nodeSelector:
    kubernetes.io/os: "linux"
  restartPolicy: "Never"
  volumes:
  - hostPath:
      path: "/var/run/docker.sock"
    name: "volume-0"
  - emptyDir:
      medium: ""
    name: "workspace-volume"
  - emptyDir: {}
    name: "jenkins-agent"
    '''
        }
    }
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
                sleep 300
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
