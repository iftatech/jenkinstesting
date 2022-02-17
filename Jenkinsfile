pipeline {
   agent any

    stages{
        stage('Build Docker Image'){
        steps{
        sh "docker build . -t gzlkhan/furiouswheel:${env.BUILD_ID}"
     }
}

        stage('Dockerhub Push'){

        steps{
        withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
        sh "docker login -u gzlkhan -p ${dockerHubPwd}"
        sh "docker push gzlkhan/furiouswheel:${env.BUILD_ID}"
        }
     }
   }
       stage('Docker Remove Image') {
      steps {
        sh "docker rmi gzlkhan/furiouswheel:${env.BUILD_NUMBER}"
      }
    }
       stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f service.yaml'
        }
      }
  }

}

}
