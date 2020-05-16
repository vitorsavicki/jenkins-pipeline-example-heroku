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
                  sh "docker login -u $USERNAME -p $PASSWORD registry.heroku.com"
               
                }
                // Tag docker img (in my case it was an image in dockerhub)
          sh "docker tag webimage:$BUILD_NUMBER registry.heroku.com/damp-bayou-27616/web"
          sh "docker push registry.heroku.com/damp-bayou-27616/web"
          sh "/usr/bin/heroku container:release web --app=damp-bayou-27616"
          sh "docker logout registry.heroku.com"
          }
        }             
    }

  }
}
