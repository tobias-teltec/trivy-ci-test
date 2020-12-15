pipeline {

  agent any

  stages {

      stage("Build image") {
            steps {
                script {
                    customImage = docker.build("trivy:${env.BUILD_ID}")
                }
            }
      }
    
      stage("Trivy Scan") {
          
                   sh '''#!/bin/sh
                   
                         export VERSION=$(curl --silent "https://api.github.com/repos/aquasecurity/trivy/releases/latest" | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/')'
                         wget https://github.com/aquasecurity/trivy/releases/download/v${VERSION}/trivy_${VERSION}_Linux-64bit.tar.gz'
                         tar zxvf trivy_${VERSION}_Linux-64bit.tar.gz'
              
                         trivy --exit-code 0 --cache-dir .trivycache/ --no-progress --severity HIGH customImage
                         '''
            }       
      }  

      stage("Push image") {
            steps {
                script {
                            customImage.push()
                    }
                }
            }
        }
    }
}