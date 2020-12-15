pipeline {
agent any

  stages {
      stage("Build image") {
          steps {
              script {
                myimage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
              }
         }
      }
      stage("Trivy Scan") {
          agent {
              docker {
                  image 'aquasec/trivy'
                  args "-v ${WORKSPACE}/trivycache/:/root/.trivycache/ --entrypoint=/bin/sh"
              }
          }
          steps {
                        sh "trivy --exit-code 0 --no-progress --severity MEDIUM,LOW tobiasparaiso/trivy:${env.BUILD_ID}"
                        sh "trivy --exit-code 1 --cache-dir .trivycache/ --severity HIGH,CRITICAL --no-progress tobiasparaiso/trivy:${env.BUILD_ID}"      
                     }
              }   
        }    
    }  
/*
      stage("Push image") {
            steps {
                script {
                            myimage.push()
                    }
                }
            }
               */
}
