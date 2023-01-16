pipeline {
  agent any
  stages {
   
   stage ('Test') { // la phase build
steps {
bat 'gradlew test'
 junit 'build/test-results/test/TEST-Matrix.xml'
 
   cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'My report',
                fileIncludePattern: 'target/report.json',
     
                trendsLimit: 10

}
}
   
   
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonar'
            }


          }
        }
   
   
         stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
   
   
     stage('build') {
     
   
      steps {
        bat(script: 'gradle build', label: 'gradle build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
       
      }
    }
   
     stage('Deploy') {
      steps {
        bat 'gradle publish'
      }
    }
   
     stage('Notification') {
      steps {
       
       notifyEvents message: 'New build is Created success', token: 'PY3Y9jJQuN-cnl4BlZV44K7bzlpd1OQg'


      }
    }
   
   
   
   
   
   
}
 
  post {

        failure {
          mail(subject: 'Build Failure', body: "the new build isn't deployed succesfully !", from: 'ja_manaa@esi.dz', to: 'ja_manaa@esi.dz')
        }
       
      }

 }
