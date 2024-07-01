pipeline {
    agent {
        label 'Jenkins-slave'
    } 
    
    environment {
        ACR_SERVER = 'docker123456789.azurecr.io'
        ACR_USERNAME = credentials('ACR')
        ACR_PASSWORD = credentials('ACR')
        IMAGE_TAG = "nginx:${BUILD_NUMBER}"
        ACR_IMAGE_TAG = "${ACR_SERVER}/nginx:${BUILD_NUMBER}"
    }
    
    stages { 
        stage('Code checkout') {
            steps {  
                git 'https://github.com/rutwikdadgal/TS-demo.git'
            }
        }
        
        stage('Build docker image') {
            steps {  
                sh "docker build -t ${IMAGE_TAG} ."
            }
        }
        
        stage('Login to ACR and push image') {
            environment {
                DOCKER_CLI_HOME = "${workspace}"
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'ACR', usernameVariable: 'ACR_USERNAME', passwordVariable: 'ACR_PASSWORD')]) {
                        // Log into ACR using Docker CLI with username and password from credentials
                        sh """
                            echo \${ACR_PASSWORD} | docker login -u \${ACR_USERNAME} --password-stdin \${ACR_SERVER}
                            docker tag ${IMAGE_TAG} ${ACR_IMAGE_TAG}
                            docker push ${ACR_IMAGE_TAG}
                        """
                    }
                }
            }
        }
    }
}
