def img

pipeline {
  agent any

  environment {
    registry = 'suyogshimpi/jenkins_docker_flask_pipeline'
    registryCredential = 'docker-hub-creds1'
    dockerImage = ''
  }

  stages {
    stage('cleanup-before-build') {
      steps {
        cleanWs()
      }
    }

    stage('checkout') {
      steps {
        sh '''
        git clone https://github.com/Evergreenies/jenkins_docker_flask_pipeline.git .
        '''
      }
    }

    stage('build') {
      steps {
        script {
          img = registry + ":${BUILD_ID}"
          println("${img}")
          dockerImage = docker.build("${img}", '-f ./Dockerfile .')
        }
      }
    }

    stage('test') {
      steps {
        sh '''
        docker run -d --name ${JOB_NAME} -p 5000:5000 ${registry}:${BUILD_ID}
        '''
      }
    }

    stage('publish') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('cleanup-after-build') {
      steps {
        cleanWs()
      }
    }
  }
}
