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
        stage('Kubernetes Setup') {
            steps {
                  git credentialsId: '5ab742e1-eb62-48ae-a975-82e6691a9f49',
                  url: 'https://github.com/kkushagra6/k8s.git',
                  branch: 'main'
                  }
        }
        stage('Update k8s mainfest file') {
          steps{
           script{
                    withCredentials([usernamePassword(credentialsId: '5ab742e1-eb62-48ae-a975-82e6691a9f49', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        cat deploy.yaml
	                sed -i 's/docker_argo_k8s:[0-9]*/docker_argo_k8s:${BUILD_NUMBER}/' deploy.yaml
                        cat deploy.yaml
                        
                        git config --global user.email "kkushagra6@gmail.com"
                        git config --global user.name "kkushagra6"
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        echo $GIT_PASSWORD
                        git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/kkushagra6/k8s.git  HEAD:main
                        '''                        
                    }
                }
          }
        }

 
    }
}
