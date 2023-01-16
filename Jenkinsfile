pipeline {
  agent any
  stages {
    stage ('test') { // la phase test
            post {
        failure {
          script {
            mail= " test termine avec echec "
          }

        }

        success {
          script {
            mail=" test termine avec succes "
          }

        }

      }
steps {
   bat 'gradle test'
   junit 'build/test-results/test/TEST-Matrix.xml'
          cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'My report',
                fileIncludePattern: 'target/*.json',
                trendsLimit: 10
  
}
    }
            stage('test Mail Notification') {
      steps {
        mail(subject: 'TPOGL Jenkins test phase notification', body: mail, cc: 'ja_manaa@esi.dz' ,bcc:'ja_manaa@esi.dz')
      }
    }
    
     
        stage('Code Analysis') {
                post {
        failure {
          script {
            mail= " Code Analysis termine avec echec "
          }

        }

        success {
          script {
            mail=" Code analysis termine avec succes "
          }

        }

      }
          
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonarQube'
            }

            waitForQualityGate true
          }
        }
            stage('analysis Mail Notification') {
      steps {
        mail(subject: 'TPOGL Jenkins analysis phase notification', body: mail, cc: 'ja_manaa@esi.dz' ,bcc:'ja_manaa@esi.dz')
      }
    }
       stage('Build') {
               post {
        failure {
          script {
            mail= " Build termine avec echec "
          }

        }

        success {
          script {
            mail=" Build termine avec succes "
          }

        }

      }
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/**'
      }
    }
            stage(' build Mail Notification') {
      steps {
        mail(subject: 'TPOGL Jenkins build phase notification', body: mail, cc: 'ja_manaa@esi.dz' ,bcc:'ja_manaa@esi.dz')
      }
    }
        stage('Deployment') {
                post {
        failure {
          script {
            mail= " Deployement terminé avec échec "
          }

        }

        success {
          script {
            mail=" Deployement termine avec succes "
          }

        }

      }
      steps {
        bat 'gradle publish'
      }
    }
        stage(' deploy Mail Notification') {
      steps {
        mail(subject: 'TPOGL Jenkins notification', body: mail, cc: 'ja_manaa@esi.dz' ,bcc:'ja_manaa@esi.dz')
      }
    }
      stage('Slack Notification') {
      steps {
        slackSend(message: 'Slack vous indique que le processus est termine avec succes. ')
      }
    }
   
}//stages

}//pipeline