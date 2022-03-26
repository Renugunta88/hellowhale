pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/Renugunta88/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("renugunta88/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '1f98bb7f-b2f4-4f95-86cb-b33149953639') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          //kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
          sh 'sed -i \'s/BUILDNUMBER/\'$BUILD_ID\'/g\' hellowhale.yml'
          sh 'cat hellowhale.yml'
          sh 'kubectl apply -f hellowhale.yml'
        }
      }
    }

  }

}
