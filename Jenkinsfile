pipeline {
   agent { label 'docker_exec' }

   stages {
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
   }
}
