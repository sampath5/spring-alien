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
          sh 'kubectl create deployment hello-spring --image=sampath5/docker-spring'
          sh 'kubectl expose deployment hello-spring --type=NodePort --port=8080'
          sh 'minikube service hello-spring'
        }
      }
      }
}
