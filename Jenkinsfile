pipeline {
agent any
environment
  IMAGE = 'tobiasparaiso/trivy:$BUILD_ID'
  stages {

      stage("Build image") {
                steps {
                  script {
                 sh 'docker build -t $IMAGE'
                }
            }
      }

      stage("Trivy Scan") {
           steps {
             script {
               sh 'wget https://github.com/aquasecurity/trivy/releases/download/v0.14.0/trivy_0.14.0_Linux-64bit.tar.gz'
               sh 'tar zxvf trivy_0.14.0_Linux-64bit.tar.gz'
               sh './trivy --exit-code 0 --cache-dir $WORKSPACE/.trivycache/ --no-progress --severity HIGH $IMAGE'           
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