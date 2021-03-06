pipeline { 
  agent any
  environment {
      EMAIL_BODY = 
      """
      <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
      <p>
      View console output at 
      "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
      </p> 
      <p><i>(Build log is attached.)</i></p>
      """
      EMAIL_SUBJECT_SUCCESS = "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 
      EMAIL_SUBJECT_FAILURE = "Status: 'FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 
      EMAIL_RECEPIENT = 'nicholas.v.ndiki@gmail.com'
  }
  tools { 
    nodejs 'Nodejs-4'
  }  
  stages { 
    stage('clone repository') {
      steps { 
        git 'https://github.com/NdiklasTheCoder/gallery-1'
      } // end of steps
    }
     stage('run test') {
      steps { 
        sh 'npm install'
        sh 'npm test'
      } 
      // run test stage
      post{
        failure{
          emailext attachLog: true,
          body: EMAIL_BODY,
          subject: EMAIL_SUBJECT_FAILURE,
          to: EMAIL_RECEPIENT
        }
        success{
          emailext attachLog: true,
          body: EMAIL_BODY,
          subject: EMAIL_SUBJECT_SUCCESS,
          to: EMAIL_RECEPIENT
        }
      }
    }
    stage('Build the project') {
      steps { 
        sh 'npm install'
      }  
    } // build project
    stage('Deploy to Heroku') {
      steps {
        withCredentials([usernameColonPassword(credentialsId: 'Ndiki', variable: 'HEROKU_CREDENTIALS' )]){
        sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/glacial-springs-58405.git master'
      }
    }
    }  
}
}

 node {
    try {
        stage 'Checkout'
            checkout scm
            sh 'git log HEAD^..HEAD --pretty="%h %an - %s" > GIT_CHANGES'
            def lastChanges = readFile('GIT_CHANGES')
            slackSend color: "warning", message: "Started `${env.JOB_NAME}#${env.BUILD_NUMBER}`\n\n_The changes:_\n${lastChanges}"
        stage 'Clone repository'
            echo 'Repository exists'
        stage 'Test'
            echo 'testing'
        stage 'Deploy'
            echo "Testing deploy."
        stage 'Publish results'
            slackSend color: "good", message: "Build successful :sunglasses: \n `${env.JOB_NAME}#${env.BUILD_NUMBER}` <${env.BUILD_URL}|Fire up Jenkins> \n Bravo Site is Live \n https://glacial-springs-58405.herokuapp.com/"
    }
    catch (err) {
        slackSend color: "danger", message: "Build failed :disappointed_relieved: \n`${env.JOB_NAME}#${env.BUILD_NUMBER}` <${env.BUILD_URL}|Fire up Jenkins>"
        throw err
    }
}
