pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t seethatgee/trio-flask-jenkins:latest ./flask-app
                docker build -t seethatgee/trio-nginx-jenkins:latest ./nginx
                docker build -t seethatgee/trio-mysql-jenkins:latest ./db
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push seethatgee/trio-flask-jenkins:latest 
                docker push seethatgee/trio-nginx-jenkins:latest 
                docker push seethatgee/trio-mysql-jenkins:latest 
                '''
            }
        }     
        stage('cleanup') {
            steps {
                sh '''
                docker system prune -a --force
                '''
            }
        }     
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s
                kubectl rollout restart deployment flask-deployment
                '''
            }
        }
    }
}