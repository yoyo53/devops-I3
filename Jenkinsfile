pipeline {
    agent any

    stages {
        stage('clone the repository') {
            steps{
                git branch: 'main', url: 'https://github.com/yoyo53/devops-I3'
            }
        }

        stage('Building image') {
            steps {
                script {
                    dir('webapi') {
                        image = docker.build("yoyo53/devops-i3")
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

        stage('Deploy to Development') {
            steps {
                script {
                    sh """
                        kubectl create namespace development
                        kubectl apply -n development -f k8s/deployment.yaml
                        kubectl wait -n development --for=condition=available --timeout=30s deployment/devops-i3
                    """
                }
            }
        }

        stage('Test in Development') {
            steps {
                script {
                    String status = sh(script: "curl -sLI -w '%{http_code}' http://\$(minikube ip):30001 -o /dev/null", returnStdout: true)
                    if (status != '200') {
                        error("Application test failed in development environment")
                    }
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    sh """
                        kubectl create namespace production
                        kubectl apply -n production -f k8s/deployment.yaml
                        kubectl wait -n production --for=condition=available --timeout=30s deployment/devops-i3
                    """
                }
            }
        }
    }
}
