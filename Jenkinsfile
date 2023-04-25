pipeline {
    
    agent any
    
    environment {
        registry = "036384991984.dkr.ecr.us-west-2.amazonaws.com/raw"
    app='account'
    AWS_ACCESS_KEY_ID     = credentials('secret_key')
    AWS_SECRET_ACCESS_KEY = credentials('Secret_access_key')
    }
    
    stages {

        stage('Clone Repo') {
            steps {
                
		git branch: 'QA', credentialsId: 'githubcred', url: 'https://github.com/vjaytech/hello-world.git'
                
            }
        }
        stage('Building image') {
            steps{
                  script {
                  dockerImage = docker.build registry + ":$BUILD_NUMBER"
                  }
            }
        }
        stage('Pushing to ECR') {
            steps{  
                  script {
                  sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 036384991984.dkr.ecr.us-west-2.amazonaws.com/raw'
                  sh 'docker tag  036384991984.dkr.ecr.us-west-2.amazonaws.com/raw:$BUILD_NUMBER'
                  sh 'docker push 036384991984.dkr.ecr.us-west-2.amazonaws.com/raw:$BUILD_NUMBER'
                  }
            }
        }
        /*stage('Cleanup docker images'){
            steps{
                sh 'docker rmi 090802525399.dkr.ecr.ap-south-1.amazonaws.com/r360bialecrepository1:$BUILD_NUMBER'
            }
        }
        stage('deploy into kubernetes Cluster') {
            steps{  
                  script {
                 // sh 'aws eks --region ap-south-1 update-kubeconfig --name R360-BIAL-CLUSTER'
                 //sh 'chmod +x changeTag.sh'
                  //sh './changeTag.sh $BUILD_NUMBER'
                  sh 'kubectl apply -f notification-service.yaml'
                 }
            }   
        }*/
    }
}
