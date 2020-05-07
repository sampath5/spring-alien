pipeline{
  environment {
    registry = "sampath5/docker-spring"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
    stages {
        stage('clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build registry + ":latest"
                }
             }
          }
          stage('Deploy Image') {
              steps{
                  script {
                      docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                      }
                  }
              }
            }
      }
}
