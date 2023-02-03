pipeline {
    environment {
        registry = 'tbermudez1215/flask_app'
        registryCredentials = 'docker'
        cluster_name = 'Skillstorm'
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
  }
}