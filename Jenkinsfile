pipeline {
agent any

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
                        sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/jenkins/trivycache/:/root/.cache/ aquasec/trivy --exit-code 0 --format template --template "@contrib/junit.tpl" -o /root/.cache/scan-report.xml tobiasparaiso/trivy:"$BUILD_ID"'
                        sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/jenkins/trivycache/:/root/.cache/ aquasec/trivy --exit-code 0 --no-progress --severity MEDIUM,LOW tobiasparaiso/trivy:"$BUILD_ID"'
                        sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/jenkins/trivycache/:/root/.cache/ aquasec/trivy --exit-code 0 --no-progress --severity CRITICAL,HIGH tobiasparaiso/trivy:"$BUILD_ID"'
                        sh 'chown -R jenkins. /var/lib/jenkins/trivycache/'
                     }
              }   
        }  

      stage("Push image") {
            steps {
                script {
                     docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myimage.push()
                            myimage.push("latest")
                    }
                }
            }
        }
    }
}
/*   
post {
        always {
              junit 'trivycache/scan-report.xml'
        }
    }
}
*/ 