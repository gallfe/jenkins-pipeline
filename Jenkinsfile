pipeline {
    environment {
      registry = "gallfe/jenkins-docker-test"
    }
    agent {
      docker {
        image 'gallfe/my-node-docker:2.0'
        args '-p 3000:3000'
        args '-w /app'
        args '-v /var/run/docker.sock:/var/run/docker.sock'
      }
  }

  options {
    skipStagesAfterUnstable()
  }

  stages {
    stage("Build"){
      steps {
        sh 'npm install'
      }
    }
    stage("Test"){
      steps {
        sh 'npm test'
      }
    }
    stage("Build & Push Docker image") {
      steps {
        sh 'docker image build -t $registry:$BUILD_NUMBER .'
        sh 'docker login -u gallfe -p N_1k0t1n'
        sh 'docker image push $registry:$BUILD_NUMBER'
        sh "docker image rm $registry:$BUILD_NUMBER"
      }
    }
    stage('Deploy and smoke test') {
      steps {
        sh './jenkins/scripts/deploy.sh'
      }
    }
    stage('Cleanup') {
      steps{
        sh './jenkins/scripts/cleanup.sh'
      }
    }
  }
}

