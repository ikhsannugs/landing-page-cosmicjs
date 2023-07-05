pipeline {
 agent any

 environment {
   GIT_COMMIT_SHORT = sh(returnStdout: true, script: '''echo $GIT_COMMIT | head -c 7''')
 }

 stages {
   stage('Prepare .env') {
     steps {
       sh 'echo GIT_COMMIT_SHORT=$(echo $GIT_COMMIT_SHORT) > .env'
     }
   }

   stage('Compile') {
      agent {
        docker {
          image 'node:13.8.0-stretch-slim'
          reuseNode true
        }
      }
      steps {
        sh 'npm install'
      }
   }

   stage('Package Docker') {
     steps {
         sh 'docker build . -t landpage:$GIT_COMMIT_SHORT'
         sh 'docker tag landpage:$GIT_COMMIT_SHORT ikhsannugs/landpage:$GIT_COMMIT_SHORT'
         sh 'docker push ikhsannugs/landpage:$GIT_COMMIT_SHORT'
       }
     }
   }
}
