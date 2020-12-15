pipeline {
agent any

  stages {
      stage("Build image") {
          steps {
              script {
                myimage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
                sh "export myimage=tobiasparaiso/trivy:${env.BUILD_ID}" 
                sh "echo ${env.myimage}"
              }
         }
      }
      stage("Trivy Scan") {
          steps {
              script {
                        sh "wget https://github.com/aquasecurity/trivy/releases/download/v0.14.0/trivy_0.14.0_Linux-64bit.tar.gz"
                        sh "tar xzvf trivy_0.14.0_Linux-64bit.tar.gz"
                        sh "./trivy --exit-code 0 --no-progress --severity HIGH ${env.myimage}"        
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
