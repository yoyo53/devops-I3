pipeline {
    
    agent any
    stages{
        stage('clone the repository'){
            steps{
                git branch: 'main', url: 'https://github.com/yoyo53/devops-I3'
                script{
                    sh "ls -l"
                }
            }
        }
    }
}