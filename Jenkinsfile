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
            steps {
                script {
                /*Baixando Trivy*/
                    sh 'export TRIVY_VERSION=$(wget -qO - "https://api.github.com/repos/aquasecurity/trivy/releases/latest" | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/')' 
                    sh 'echo $TRIVY_VERSION'
                    sh 'wget --no-verbose https://github.com/aquasecurity/trivy/releases/download/v${TRIVY_VERSION}/trivy_${TRIVY_VERSION}_Linux-64bit.tar.gz -O - | tar -zxvf -'
                /*Executando Trivy*/    
                    sh 'trivy --exit-code 0 --cache-dir .trivycache/ --no-progress --severity HIGH customImage'
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