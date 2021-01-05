pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
        
        imagename = "johnoverboard/hello-world-1"
        registryCredential = 'ca9400ea-ddce-414a-a821-55a0daf01fc0'
        dockerImage = ''

    }
    stages {
        stage("clone code"){
            steps{
               git credentialsId: 'git_credentials', url: 'https://github.com/joelvzach/hello-world-1.git'
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
        
        stage('Upload Image') {
        steps{    
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
                }
            }
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
