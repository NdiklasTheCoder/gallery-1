pipeline { 
  agent any

//    environment {

//         EMAIL_BODY = 

//         """

//             <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>

//             <p>

//             View console output at 

//             "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"

//             </p> 

//             <p><i>(Build log is attached.)</i></p>

//         """

//         EMAIL_SUBJECT_SUCCESS = "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 

//         EMAIL_SUBJECT_FAILURE = "Status: 'FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'" 

//         EMAIL_RECEPIENT = 'nicholas.v.ndiki@gmail.com'

//     }

  tools { 
    gradle "Gradle-6"
  }  
  stages { 
    stage('clone repository') {
      steps { 
        git 'https://github.com/NdiklasTheCoder/gallery-1'
      } // end of steps
    }
    stage('Build the project') {
      steps { 
        sh 'gradle build'
      }  
    } // build project
   stage('Deploy to Heroku') {
     steps {
      withCredentials([usernameColonPassword(credentialsId: 'Ndiki', variable: 'HEROKU_CREDENTIALS' )]){
      sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/immense-fjord-65904.git master'
    }
  }

// } // test stage
//     stage('Tests') {
//       steps { 
//         sh 'gradle test'
//       }
//   //post stage build inside a stage
//       }
//     }
//   }
