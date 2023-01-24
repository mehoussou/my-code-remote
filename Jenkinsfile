registry = "public.ecr.aws/v3f8i0q6/back-app"

pipeline {
    agent none
  stages {
    stage ('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/pipeline']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mehoussou/my-remote-code.git']])
                
            }
        }
    }


    stage("Build Image"){
        agent any
        steps{
            echo "registry"
            echo "service"
            echo  "registry"+ "/" + "service" + ":rc-" + BUILD_NUMBER
            sh " docker build . -t  ${registry}/${service}:rc-${BUILD_NUMBER}"
        }
    }
    stage('Login to ecr') {
        agent any
        steps {
            script{
                echo service
                withAWS(region:'us-east-2', credentials:'ecr-creds') {
                    sh " aws ecr get-login-password | docker login -u AWS --password-stdin  ${registry}"
                }
            }
        }
    }
    stage('push image') {
        agent any
        steps {
            script{
                echo "$service"
                withAWS(region:'us-east-2', credentials:'ecr-creds') {
                    sh "docker push ${registry}/${service}:rc-${BUILD_NUMBER} "
                }
            }
        }
    }

    stage ('test code'){
        steps {
            sh 'docker-compose -f docker-compose.yml up -d '
        }
    }
}