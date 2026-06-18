pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Avishka6/nodeapp.git'
                }
            }
        }
        stage('Cleanup') {
    steps {
        bat 'docker system prune -a -f'
    }
}
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t avishka6/nodeapp-test:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerpassword', variable: 'testdockerhubpass')]) {
                    script {
                        bat "docker login -u avishka6 -p %testdockerhubpass%"
                    }
                }
            }
        }
        stage('Push Image') {
    steps {
        script {
            retry(3) {
                bat "docker push avishka6/nodeapp-test:${env.BUILD_NUMBER}"
            }
        }
    }
}
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
