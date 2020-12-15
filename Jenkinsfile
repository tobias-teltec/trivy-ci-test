pipeline {

  agent any

  stages {

      stage("Build image") {
            steps {
                  def customImage = docker.build("tobiasparaiso/trivy:${env.BUILD_ID}")
                }
            }
    
      stage("Trivy Scan") {
          agent {
              docker {
                  image 'aquasec/trivy'
                  args '-v $WORKSPACE/trivy:/root/.trivycache --entrypoint='
              }
          }
          steps {
                   sh 'trivy --exit-code 0 --cache-dir /root/.trivycache/ --no-progress --severity HIGH customImage'           
            }   
        }      

      stage("Push image") {
            steps {
                            customImage.push()
                    }
                }
            }
        }