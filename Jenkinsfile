pipeline {
    agent {label "Node2qa"}             # To run our Application on Perticuler Node.
    environment {
    DOCKERHUB_CREDENTIALS = credentials('valaxy-dockerhub')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git credentialsId: 'javahome2', url: 'https://github.com/Thimraj/nodejs-demo-for-docker.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t dockerthimraj/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push dockerthimraj/nodeapp:$BUILD_NUMBER'
            }
        }
        stage("Create Container") {
            steps{
                sh 'docker stop mynodeappcont1'         # To build again and again to without fail
                sh 'docker rm mynodeappcont1'           # To build again and again to without fail
                sh 'docker run -itd --name mynodeappcont1 -p 3600:3000 dockerthimraj/nodeapp:${BUILD_NUMBER}'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}
