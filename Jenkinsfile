pipeline {
    agent {
        label 'Jenkins-slave'
    } 
    
    environment {
        ACR_SERVER = 'docker123456789.azurecr.io'
        ACR_USERNAME = credentials('ACR')
        ACR_PASSWORD = credentials('ACR')
        DOCKER_IMAGE_TAG = 'latest' // Set the tag you want to use for the latest build
    }
    
    stages { 
        stage('Code checkout') {
            steps {  
                git 'https://github.com/rutwikdadgal/TS-demo.git'
            }
        }
        
        stage('Build docker image') {
            steps {  
                sh "docker build -t $ACR_SERVER/nginx:$BUILD_NUMBER ."
            }
        }
        
        stage('Login to ACR and push image') {
            environment {
                DOCKER_CLI_HOME = "${workspace}"
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'ACR', usernameVariable: 'ACR_USERNAME', passwordVariable: 'ACR_PASSWORD')]) {
                        sh """
                            echo \${ACR_PASSWORD} | docker login -u \${ACR_USERNAME} --password-stdin \${ACR_SERVER}
                            docker tag $ACR_SERVER/nginx:$BUILD_NUMBER $ACR_SERVER/nginx:$DOCKER_IMAGE_TAG
                            docker push $ACR_SERVER/nginx:$DOCKER_IMAGE_TAG
                        """
                    }
                }
            }
        }
    }
}
//using git repo
