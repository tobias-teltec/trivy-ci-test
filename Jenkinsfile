pipeline {
agent any

  stages {
      stage("Build image") {
          steps {
                customImage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
              }
      }
      stage("Trivy Scan") {
          steps {
                    customImage.inside {
                        sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/master/contrib/install.sh | sh -s -- -b /usr/local/bin'
                        sh 'trivy fs /'        
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
}