pipeline {
    agent any

    stages {
        stage('clone the repository') {
            steps{
                git branch: 'main', url: 'https://github.com/yoyo53/devops-I3'
            }
        }
    }
    stage('Building image') {
        steps {
            script {
                dir('webapi') {
                    image = docker.build("yoyo53/devops-I3")
                }
            }
        }
    }
    stage('Publish Image') {
        steps {
            script {
                docker.withRegistry('https://ghcr.io', 'ghcr') {
                    image.push('latest')
                }
            }
        }
    }
}
