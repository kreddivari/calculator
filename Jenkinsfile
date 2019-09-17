pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
     
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }

          stage("Docker build") {
               steps {
                    sh "docker build -t kreddiva/pipeline:${BUILD_TIMESTAMP} ."
               }
          }

          stage("Docker login") {
               steps {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-credentials',
                               usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                         sh "docker login --username $USERNAME --password $PASSWORD"
                    }
               }
          }

          stage("Docker push") {
               steps {
                    sh "docker push kreddiva/pipeline:${BUILD_TIMESTAMP}"
               }
          }

         
          
         
     }
}
