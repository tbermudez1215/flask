pipeline {
    environment {
        registry = 'tbermudez1215/flask_app'
        registryCredentials = 'docker'
        cluster_name = 'Skillstorm'
        namespace = tbermudez1215
    }
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/tbermudez1215/flask', branch: 'main')
      }
    }
stage('Build Stage') {
    steps {
        script {
            dockerImage = docker.build(registry)
        }
    }
}
stage('Deploy Stage') {
    steps {
        script {
            docker.withRegistry('', registryCredentials) {
                dockerImage.push()
            }
        }
    }
}
stage('Kubernetes') {
  steps {
    withCredentials ([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsID: 'AWS', secretKeyVariable: 'AWS_SECRET_ACCES_KEY')]) {
      sh "aws eks update-kubeconfig --region us-east-1 --name ${cluster_name}"
      script{
        try{
            sh "kubectl create namespace ${namespace}"
          }catch (Exception e) {
              echo "Exeption handled"
            }
            }
            sh "kubectl apply -f deployment.yml -n ${namespace}"
            sh "kubectl -n ${namespace} rollout restart deployment flaskcontainer"
          }
        }
    }
    }
}