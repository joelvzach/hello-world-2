pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
        
        imagename = "johnoverboard/hello-world-2"
        // registryCredential = 'ca9400ea-ddce-414a-a821-55a0daf01fc0'
        registryCredential = '57044ad3-537a-440b-9253-787599bb380e'
        dockerImage = ''
        
        // ECRURL = '181405805042.dkr.ecr.ap-south-1.amazonaws.com/final-project:latest'
        // ECRRED = ''

    }
    stages {
        stage("clone code"){
            steps{
               git credentialsId: 'git_credentials', url: 'https://github.com/joelvzach/hello-world-2.git'
            }
        }
        stage("build code"){
            steps{
              sh "mvn clean install"
              
            }
        }
        stage('Building docker image') {
            steps{
                  // sh "service docker start"
                  script {
                        
                        dockerImage = docker.build imagename
                    }
            }
        }
        
        /* 
        stage('Upload Image') {
        steps{    
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
                }
            }
        }
      }
      */
      
        stage('Upload to ECR') {
            steps {
                sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 181405805042.dkr.ecr.ap-south-1.amazonaws.com"
                sh "docker tag final-project:latest 181405805042.dkr.ecr.ap-south-1.amazonaws.com/final-project:latest"
                sh "docker push 181405805042.dkr.ecr.ap-south-1.amazonaws.com/final-project:latest"
              
            }
        }
        
        
       /* stage("deploy"){
            steps{
              sshagent(['deploy_user']) {
                 sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@13.229.183.126:/opt/apache-tomcat-8.5.55/webapps"
                 
                }
            }
        }*/
    }
}
