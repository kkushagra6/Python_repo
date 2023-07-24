pipeline {
    
    agent any 
    
    environment {
        IMAGE_VERSION = "${BUILD_NUMBER}"
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
    }
}
