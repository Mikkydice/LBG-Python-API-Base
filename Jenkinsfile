pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.200.0.7 << EOF
                docker stop flask-app || echo "flask-app Not Running"
                docker rm flask-app || echo "flask-app Not Running"
                
                '''
           }
        }

        stage('Build') {
            steps {
                sh '''
                docker build -t mikkydice/python-api -t mikkydice/python-api:v${BUILD_NUMBER} .
                
                '''
           }
        }

        stage('Push') {
            steps {
                sh '''
                docker push mikkydice/python-api
                docker push mikkydice/python-api:v${BUILD_NUMBER}
                '''
           }
        }

        stage('Deploy') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.200.0.7 << EOF 
                docker run -d -p 80:8080 --name flask-app mikkydice/python-api 
                '''
           }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker system prune -f
                
                '''
            }
        }
    }
}
