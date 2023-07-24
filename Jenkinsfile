pipeline {
    
    agent any 
    
    environment {
        IMAGE_VERSION = "${BUILD_NUMBER}"
        DOCKER_REGISTRY_CREDENTIALS = credentials('docker-registry-credentials')
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                sh '''
                ls -lrt
                '''
           }
        }
        stage("Docker Build"){
          steps{
            sh '''
            echo "Building Docker Image"
            docker build -t kkushagra6/docker_argo_k8s:${IMAGE_VERSION} .
            ls -lrt
            '''
          }
        }
        stage("Pushing to DockerHub"){
          steps {
          sh '''
          docker login -u ${DOCKER_REGISTRY_CREDENTIALS_USR} -p ${DOCKER_REGISTRY_CREDENTIALS_PSW} docker.io
          docker push kkushagra6/docker_argo_k8s:${IMAGE_VERSION}
          '''
          }
        }



 
    }
}
