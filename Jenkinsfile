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
        sh 'cd spring-boot-package-war && mvn compile'
      }
    }

    stage('Test the App') {
      steps {
        sh 'cd spring-boot-package-war && mvn test'
      }
    }

    stage('Change Version') {
      steps {
        sh '''cd spring-boot-package-war && mvn versions:set versions:commit -DnewVersion="0.0.1.$BUILD_NUMBER"





 '''
        sh '''git config --global user.name aviadhager
git config --global user.email "aviadhager@gmail.com"
git add spring-boot-package-war/pom.xml 
git commit -m "Commit the new version number to the .pom file" spring-boot-package-war/pom.xml'''
        sh '''withCredentials([usernamePassword(credentialsId: \'github\', passwordVariable: \'tchgsar3\', usernameVariable: \'aviadhager\')]) {
                        sh(\'git push https://github.com/aviadhager/spring-boot-examples.git\')
                    }'''
        }
      }

      stage('Clean Package') {
        steps {
          sh 'cd spring-boot-package-war && mvn clean package'
          archiveArtifacts(artifacts: 'spring-boot-package-war/target/*.war', onlyIfSuccessful: true)
        }
      }

      stage('Notify Slack') {
        steps {
          slackSend(color: '#3EA652', message: "Success: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
      }

    }
  }