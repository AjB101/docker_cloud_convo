pipeline {
  agent any

  environment {
       imagename = "ajb101/ajbtomcatimage"
       registryCredential = 'DockerHub'
       dockerImage = ''
           }

  
  stages {

    stage ('Build') {
      steps {
        sh 'mvn clean package'
        echo 'Maven Build'
      }
    }

    stage('Build Test') {
            steps {
                sh 'mvn test'
                echo 'Test Analysis'
            }
        }
    
      stage('Build Docker image') { 
          steps{
                script {
                   dockerImage = docker.build imagename + ":$BUILD_NUMBER"
                  
                          } 
                      }
                }

     stage('Push Docker Image to DockerHub') {
           steps{
               script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                                              }
                                    }
                             }
                  }
     stage('Remove Unused docker image') {
          steps{
              sh "docker rmi $imagename:$BUILD_NUMBER"
              sh "docker rmi $imagename:latest"
                        }
            }  
   }
}
