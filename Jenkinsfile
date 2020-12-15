pipeline {
agent docker

  stages {

      stage("Build image") {
                steps {
                script {
                  customImage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
                }
            }
      }

      stage("Trivy Scan") {
          agent {
              docker {
                  image 'aquasec/trivy'
                  args '-v $WORKSPACE/trivy:/root/.trivycache -e customImage=$customImage'
              }
          }
          steps {
                   sh 'trivy --exit-code 0 --cache-dir /root/.trivycache/ --no-progress --severity HIGH customImage'           
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