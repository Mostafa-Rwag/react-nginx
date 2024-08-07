pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/origin']],
                          userRemoteConfigs: [[
                              url: 'https://github.com/docker/awesome-compose/tree/master/react-nginx',
                              
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
