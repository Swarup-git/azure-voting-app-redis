pipeline {
   agent { label 'docker_exec' }

   stages {
      stage('Echo- Declarative pipleline example) {
            when { branch pattern: "\\w+\\-pipeline", comparator: "REGEXP"}
            steps {
               echo 'This is a pipeline branch'
            }
      }
      stage('Echo- Scripted pipleline example) {
            steps {
               script {
                  if (env.BRANCH_NAME == 'master') {
                     echo 'This is the master branch'
                  } else {
                     echo 'This is not the master branch'
                  }
               } 
            }
      }
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
               cd azure-vote/
               docker images -a
               docker build -t jenkins-pipeline .
               docker images -a
               cd ..
            """)
         }
      }
      stage('Approve PROD Deploy') {
         when {
            branch 'master'
         }
         options {
            timeout(time: 1, unit: 'HOURS') 
         }
         steps {
            input message: "Deploy?"
         }
         post {
            success {
               echo "Production Deploy Approved"
            }
            aborted {
               echo "Production Deploy Denied"
            }
         }
      }
   }
}
