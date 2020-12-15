pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/tobias-teltec/trivy-ci-test.git', branch:'main'
      }
    }

      stage("Build image") {
            steps {
                script {
                    customImage = docker.build("trivy:${env.BUILD_ID}")
                }
            }
      }
    
      stage("Trivy Scan") {
            agent {
                docker {
                    image 'aquasec/trivy'
                    args '-v $WORKSPACE/trivycache/:/root/.trivycache --cache-dir /root/.trivycache --no-progress --severity HIGH customImage'
                }
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