#!groovy

pipeline {
  agent {
    kubernetes {
      cloud 'icp-global'
      defaultContainer 'node'
      yamlFile 'buildPod.yaml'
    }
  }
  environment {
    GIT_SSH_COMMAND = "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '10'))
    timeout(time: 15, unit: 'MINUTES')
    timestamps()
    skipStagesAfterUnstable()
  }

  stages {
    stage('setup') {
      steps {
        sh 'yarn install'
      }
    }

    stage('db') {
      steps {
        container('mysql') {
          sh 'find /docker-entrypoint-initdb.d'
        }
      }
    }
  }
}
