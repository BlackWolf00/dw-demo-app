pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withMaven(maven: 'maven3') {
          sh 'mvn compile'
        }

      }
    }
    stage('Unit Tests') {
      steps {
        withMaven(maven: 'maven3') {
          sh 'mvn test'
        }

      }
    }
    stage('Package') {
      steps {
        withMaven(maven: 'maven3') {
          sh 'mvn package'
        }

        stash(name: 'dockerfile', includes: 'Dockerfile')
        stash(name: 'config', includes: 'hello-world.yml')
      }
    }
    stage('Integration Tests') {
      steps {
        withMaven(maven: 'maven3') {
          sh 'mvn verify'
        }

      }
    }
    stage('Docker build') {
      steps {
        node(label: 'docker') {
          unstash 'config'
          unstash 'dockerfile'
          unstash 'app'
        }

      }
    }
  }
  environment {
    artefactName = 'prova'
  }
}