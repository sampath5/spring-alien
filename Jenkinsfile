pipeline{
  environment {
    registry = "sampath5/springalien"
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
        stage('build and test') {
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
          stage('Push Image') {
              steps{
                  script {
                      docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                      }
                  }
              }
            }
      stage('Deploying into k8s'){
        steps{
          sh 'kubectl apply -f deployment.yml'
          sh 'kubectl apply -f service.yml'
        }
      }
      }
}
