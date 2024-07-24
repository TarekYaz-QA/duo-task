pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -f Dockerfile.flask -t tarekyaz/duo-deploy-flask:latest -t tarekyaz/duo-deploy-flask:v$BUILD_NUMBER .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push tarekyaz/duo-deploy-flask:latest
                docker push tarekyaz/duo-deploy-flask:v$BUILD_NUMBER
                '''
            }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker rmi tarekyaz/duo-deploy-flask:latest
                docker rmi tarekyaz/duo-deploy-flask:v$BUILD_NUMBER
                ''' 
            } 
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments
                kubectl rollout restart deployment flask-deployment
                '''
            }
        }
    }
}
