pipeline {
  environment {
    imagename = "dwip23/jenkins-node-aap"
    registryCredential = 'docker-cred'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/hazradwip/nodeJS-web-app.git', branch: 'master', credentialsId: 'github-token'])
 
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }
    stage('Deploy updated docker image.') {
      steps{
        sh "docker stop node-app"
        sh "docker rm node-app"
        sh "docker pull dwip23/jenkins-node-aap:$BUILD_NUMBER
        sh "docker run -d --name node-app -p 3000:8080 dwip23/jenkins-node-aap:$BUILD_NUMBER"
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:latest"
      }
    }
  }
}
