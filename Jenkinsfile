pipeline {
    agent any

    tools {
        maven 'Maven3' // This must match what you've configured under Jenkins -> Global Tool Configuration
    }

    stages {
        stage('Build Maven') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/nandithaajay07/devops-automation']]
                ])
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t nandithaajay07/devops-integration .'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u nandithaajay07 -p ${dockerhubpwd}'
                    }
                    sh 'docker push nandithaajay07/devops-integration'
                }
            }
        }
    }
}
