pipeline {
  agent any

  stages {

    stage('Build App') {
      steps {
            echo "Building with yarn"
            sh "yarn"
            sh "yarn build"
      }
    }


    stage('Build And Push Container Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
	    sh "docker build -t docker.io/mahmut25/edutest-app:$GIT_COMMIT ."
            sh "docker push docker.io/mahmut25/edutest-app:$GIT_COMMIT"

        }
      } 
    }

  stage('Deploy to K8s') {
    steps {
      withKubeConfig([credentialsId: 'kubeconfig-credential2']) {
        sh 'helm upgrade -i edutest-app helm -f helm/values.yaml --set image.tag=$GIT_COMMIT'
      }
    }
    }
  }
}
