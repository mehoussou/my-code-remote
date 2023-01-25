pipeline {
    agent any
    
    stages {
        
        stage('Logging into AWS ECR'){
            steps {
                script {
                    
                    sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 162816112568.dkr.ecr.us-east-2.amazonaws.com'
                }
            }
        }
   
        stage ('Cloning Git') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mehoussou/my-code-remote.git']])
            }
         }
            
        stage ('Building images.....'){
            steps {
                script {
                    sh 'docker-compose.build 162816112568.dkr.ecr.us-east-2.amazonaws.com/my-code-chall:latest'
                }
            }
        }
         
         //Uploading Docker images into AWS ECR
         
        stage ('Tagging and Pushing Imagesto ECR....'){
            steps {
                script {
                    sh 'docker tag my-code-chall:latest 162816112568.dkr.ecr.us-east-2.amazonaws.com/my-code-chall:latest'
                    sh 'docker push 162816112568.dkr.ecr.us-east-2.amazonaws.com/my-code-chall:latest'
                }
            }  
        }
         
         
        stage ('Deploying Application.....'){
            steps {
                script {
                    def dockerCmd = 'docker-compose -d -p 3000:3000 --rm --name 162816112568.dkr.ecr.us-east-2.amazonaws.com/my-code-chall:latest'
                    sshagent(['lightfeatherWeb-srv-key']) {
                        sh "ssh -o StrictHostkeyChecking=no ec2-user@3.137.207.83 ${dockerCmd}"
    
                    }
                   
                }
            }
        }
    }  
}

