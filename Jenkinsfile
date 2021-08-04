pipeline {
  agent {
    node {
      label 'centof7'
    }

  }
  stages {
    stage('CheckOut') {
      steps {
        git(url: 'https://github.com/aviadhager/spring-boot-examples.git', branch: 'aviad_sol', credentialsId: 'github')
      }
    }

    stage('Build') {
      steps {
        sh 'mvn compile'
      }
    }

  }
}