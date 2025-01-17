pipeline {
   agent { label 'docker_exec' }

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Echo- Declarative pipleline example') {
            when { branch pattern: "\\w+\\-master", comparator: "REGEXP"}
            steps {
               echo 'This is a pipeline/master branch'
            }
      }
      stage('Echo- Scripted pipleline example') {
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
      stage('Push Container'){
         steps{
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote"){
               script{
                  docker.withRegistry('https://index.docker.io/v1/','forDockerHub'){
                     def image = docker.build('cazdock/jenkinspluralsight:latest')
                     image.push()
                  }
               }
            }
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
