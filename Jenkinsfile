#!groovy
pipeline {
  agent {
      docker {
          image 'maven:3-alpine'
          args '-v /root/.m2:/root/.m2'
      }
  }
   stages {     
    stage('Maven Package') {
      steps {
        sh 'mvn -DskipTests clean package'
      }
     }
     stage('Maven Tests') {
        steps {
            sh 'mvn test'
        }
        post {
            always {
                junit 'target/surefire-reports/*.xml'
            }
        }
      }
     stage('Maven Build') {
         agent {
            docker {
                image 'maven:latest'
                args '-v /root/.m2:/root/.m2 -v /var/run/docker.sock:/var/run/docker.sock' //here we expose docker socket to container. Now we can build docker images in the same way as on host machine where docker daemon is installed
            }
        }
        steps {
            sh 'mvn -Ddocker.skip=false -Ddocker.host=unix:///var/run/docker.sock docker:build'
            echo "Running ${env.BUILD_ID} on ${env.NODE_NAME}"
            input message: 'Voulez-vous continuer le build? (Cliquer sur "Aller" pour continuer)'
        }
      }
   }
 }
