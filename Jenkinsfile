#!groovy
pipeline {
  agent {
      docker {
          image 'node:6-alpine'
          args '-p 3000:3000'
      }
  }
  environment {
            CI = 'true'
    }
   stages {     
    stage('Maven Install') {
      agent {        
       docker {          
         image 'maven:3.5.0'         
        }       
      }      
      steps {
        sh 'mvn clean install'
      }
     }
     stage('Deliver') {
        steps {
            sh 'mvnw package'
            echo "Running ${env.BUILD_ID} on ${env.NODE_NAME}"
            input message: 'Voulez-vous continuer le build? (Cliquer sur "Aller" pour continuer)'
            sh './jenkins/scripts/kill.sh'
        }
      }
   }
 }
