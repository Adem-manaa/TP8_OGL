//////
pipeline {
    agent any
    stages {
        stage ('test') { // la phase build is
            steps {
                bat 'gradle test'
                archiveArtifacts 'build/test-results/'
                cucumber reportTitle: 'Cucumber report',
                fileIncludePattern: 'target/report.json',
                trendsLimit: 10,

                junit 'build/test-results/test/TEST-Matrix.xml'
            }
         }

          stage ('Code Analysis') { // la phase build
            steps {
                                withSonarQubeEnv('sonar'){
                bat 'gradle sonarqube'
                                }
            }
         }
          stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
                  stage("Build") {
            steps {
                bat 'gradle build'
                bat 'gradle javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }
             stage("deploy") {
            steps {
                bat 'gradle publish'

            }
        }
                         stage("notification") {
            steps {
                 notifyEvents message: 'Pipeline <b> is sucessufuly termined</b>', token: 'PY3Y9jJQuN-cnl4BlZV44K7bzlpd1OQg'

            }
        }

    }
            post {

        failure {
            mail bcc: '', body: '''process Failed!!!!
Sorry adam''', cc: '', from: '', replyTo: '', subject: 'process Faild', to: 'ja_manaa@esi.dz'
        }

}

    }