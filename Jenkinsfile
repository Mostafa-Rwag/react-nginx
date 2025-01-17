pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[
                              url: 'https://github.com/Mostafa-Rwag/react-nginx',
                              credentialsId: 'github-credentials'
                          ]]
                ])
            }
        }
        stage('Build Docker Image') {
            steps {
                dir('react-nginx') {
                    script {
                        def imageName = 'mostfarwag/react-nginx'
                        sh "docker build -t ${imageName} ."
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    def imageName = 'mostfarwag/react-nginx'
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        sh "docker push ${imageName}"
                    }
                }
            }
        }
    }
}
