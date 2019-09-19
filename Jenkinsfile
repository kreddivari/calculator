pipeline {
      environment {
    registry = "https://hub.docker.com/r/kreddiva/pipeline"
    registryCredential = 'docker-hub-credentials'
    dockerImage = ''
  }
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
     
       stage("Compile") {
               steps {
                    bat "./gradlew compileJava"
               }
          }
          stage("Unit test") {
               steps {
                    bat "./gradlew test"
               }
          }
          stage("Code coverage") {
               steps {
                    bat "./gradlew jacocoTestReport"
                    bat "./gradlew jacocoTestCoverageVerification"
               }
          }
          stage("Static code analysis") {
               steps {
                    bat "./gradlew checkstyleMain"
               }
          }
          stage("Package") {
               steps {
                    bat "./gradlew build"
               }
          }

        //  stage("Docker build") {
              // steps {
                  //  bat "docker build -t kreddiva/pipeline:demo ." 
             //  }
         // }
 stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
      /*    stage("Docker login") {
               steps {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-credentials',
                               usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                         bat "docker login --username $USERNAME --password $PASSWORD"
                    }
               }
          }

          stage("Docker push") {
               steps {
                    bat "docker push kreddiva/pipeline:demo"
               }
          }*/
           stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }

         
          
         
     }
}
