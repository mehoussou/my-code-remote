
// // registry = ""

// // pipeline {
// //     agent none
// //   stages {
// //     stage("Build Image"){
// //         agent any
// //         steps{
// //             echo registry
// //             echo 
// //             echo  registry + "/" + service + ":rc-" + BUILD_NUMBER
// //             sh " docker build . -t  ${registry}/${service}:rc-${BUILD_NUMBER}"
// //         }
// //     }
// //     stage('Login to ecr') {
// //         agent any
// //         steps {
// //             script{
// //                 echo service
// //                 withAWS(region:'us-east-2', credentials:'ecr-creds') {
// //                     sh " aws ecr get-login-password | docker login  -u
// // AWS --password-stdin  ${registry}"
// //                 }
// //             }
// //         }
// //     }
// //     stage('push image') {
// //         agent any
// //         steps {
// //             script{
// //                 echo service
// //                 withAWS(region:'us-east-2', credentials:'ecr-creds') {
// //                     sh "docker push ${registry}/${service}:rc-${BUILD_NUMBER} "
// //                 }
// //             }
// //         }
// //     }
// //     stage('deploy to ecs') {
// //         agent any
// //         steps {
// //             script{

// //             }
// //         }
// //     }
// //     }
// // }



// pipeline {
    
//     agent any
    
//     environment {
//         registry = "public.ecr.aws/v3f8i0q6/backend-app"
//         // registry = "public.ecr.aws/v3f8i0q6/frontend-app"
        
//     }
    
//     stages {
        
//         stage ('checkout') {
//             steps {
//                 checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mehoussou/my-code-remote-pub']])
                
//             }
//         }
                      
//         stage ('Docker Build') {
//             steps {
//                     script {
//                         dockerImage = docker.build registry
//                     }
                
//             }
               
//         }

//         stage ('Docker Push') {
//             steps {
//                 script {
//                         sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/v3f8i0q6'
//                         sh 'docker push public.ecr.aws/v3f8i0q6/backend-app:latest'
//                 }
//             }

//         }
//             // Stoping Docker containers for cleaner Docker run
//         stage ('Stop previous containers') {
//             steps {
//                 sh 'docker ps -f name=backend-app -q | xargs --no-run-if-empty docker container stop'
//                 sh 'docker container ls -a -fname=backend-app -q | xargs -r docker container rm'
//             }
//         }

//         stage ('Docker run') {
//             steps {
//                 script {
//                     sh 'docker run -d -p 3000:3000 --rm --name backend-app public.ecr.aws/v3f8i0q6/backend-app:latest'
//                 }
//             }
//         }
//     }
// }


pipeline {
  agent any
    
  tools {
    nodejs 'node.js 16'
    }
    
  stages {
        
    stage('Git') {
      steps {
        
      }
    }
     
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'cd /home/mc/lightfeather-local/backend'
        sh 'docker build -t myfront:1.0 .'
        sh 'cd /home/mc/lightfeather-local/frontend'
        sh 'docker build -t myback:1.0 .'
      }
    }  
    
            
    stage('Test') {
      steps {
        sh 'node test'
      }
    }
  }
}