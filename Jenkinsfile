pipeline {
agent any

  stages {

      stage("Build image") {
                steps {
                  script {
                  customImage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
                }
            }
      }

      stage("Trivy Scan") {
           steps {
             script {
               sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/master/contrib/install.sh | sh -s -- -b /usr/local/bin'
               sh 'trivy --exit-code 0 --cache-dir $WORKSPACE/.trivycache/ --no-progress --severity HIGH ${env.customImage}'           
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