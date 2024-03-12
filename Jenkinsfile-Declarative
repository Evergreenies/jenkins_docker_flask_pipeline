pipeline {
  agent any
  stages {
    stage('git-checkout') {
      steps {
        sh 'https://github.com/Evergreenies/jenkins_docker_flask_pipeline.git'
      }
    }

    stage('build') {
      steps {
        script {
          img = registry + ":${BUILD_ID}"
          println("${img}")
          dockerImage = docker.build("${img}")
        }

      }
    }

    stage('test') {
      steps {
        sh 'docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}'
      }
    }

    stage('publish') {
      steps {
        script {
          docker.withRegistry("https://registry.hub.docker.com", registryCredential) {
            dockerImage.push()
          }
        }

      }
    }

  }
  environment {
    registry = 'suyogshimpi/jenkins_docker_flask_pipeline'
    registryCredential = 'docker-hub-creds1'
    dockerImage = ''
  }
}