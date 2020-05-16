pipeline{
  agent any
  stages{
    stage('Compile'){
      agent any
      steps{
        sh 'echo Install Dependencies'
      }       
    }
    stage('Code Quality'){
      agent any
      steps{
        sh 'echo Sonarqube Code Quality Check Done'
      }
    }
    stage('Test'){
      agent any
      steps{
        sh 'echo Test Python'
      }
    } 
    stage('Package'){
      agent any
      steps{
        sh 'echo Package Artifactory'
      }
    }
    stage('Upload File To Artifactory'){
      agent any
      steps{
        sh 'echo Uploaded file to Artifactory'
      }
    }
     stage('Build'){
      agent any
      steps{
        sh label: '', script: '''
        sudo docker build -t webimage:$BUILD_NUMBER .'''

      }
    }

  stage('Deploy to Heroku') {
      steps {
         script {
          withCredentials([[$class: 'UsernamePasswordMultiBinding',
              credentialsId: 'heroku',
                usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                  sh "echo 'machine git.heroku.com login $USERNAME password $PASSWORD' > ~/.netrc"
                  sh "chmod 600 ~/.netrc"
                  sh "heroku git:remote -a damp-bayou-27616"
                  sh "git push -f https://git.heroku.com/damp-bayou-27616.git HEAD:master"
                }
          }
        }             
    }

  }
}
