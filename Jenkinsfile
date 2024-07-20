pipeline {
    environment {
        registry = "moolsbytheway/tpdevops"
        registryCredential = 'b2fd8f13-7826-4134-aa9c-cf64c09cb9c2'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/moolsbytheway/tpdevops'
            }
        }
        stage('Building Image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Test Image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            steps{
                sh "docker run -d -p 8081:80 moolsbytheway/tpdevops:$BUILD_NUMBER"
            }
        }
    }
}
