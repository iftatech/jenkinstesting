pipeline {
   agent any

    stages{
        stage('Build Docker Image'){
        steps{
        sh "docker build . -t ifta4766/furiouswheel:${env.BUILD_ID}"
     }
}

        stage('Dockerhub Push'){

        steps{
        withCredentials([string(credentialsId: 'docker_cred', variable: 'dockerHubPwd')]) {
        sh "docker login -u ifta4766 -p ${dockerHubPwd}"
        sh "docker push ifta4766/furiouswheel:${env.BUILD_ID}"
        }
     }
   }
       stage('Docker Remove Image') {
      steps {
        sh "docker rmi ifta4766/furiouswheel:${env.BUILD_NUMBER}"
      }
    }
       stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'azure_kubeconfig']) {
          sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f service.yaml'
        }
      }
  }

}

}
