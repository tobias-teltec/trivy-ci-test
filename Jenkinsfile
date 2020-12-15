pipeline {
agent any

environment {
    VERSION= '''
    (curl --silent "https://api.github.com/repos/aquasecurity/trivy/releases/latest" | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/')
    '''
}
  stages {
      stage("Build image") {
          steps {
              script {
                myimage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
              }
         }
      }
      stage("Trivy Scan") {
          steps {
              script {
                        sh "wget https://github.com/aquasecurity/trivy/releases/download/v${VERSION}/trivy_${VERSION}_Linux-64bit.tar.gz"
                        sh "tar zxvf trivy_${VERSION}_Linux-64bit.tar.gz"
                        sh "./trivy --exit-code 0 --no-progress --severity HIGH tobiasparaiso/trivy:${env.BUILD_ID}"
                        sh "./trivy --exit-code 1 --cache-dir .trivycache/ --severity CRITICAL --no-progress tobiasparaiso/trivy:${env.BUILD_ID}"      
                     }
              }   
        }    
    }  
/*
      stage("Push image") {
            steps {
                script {
                            customImage.push()
                    }
                }
            }
               */
}
