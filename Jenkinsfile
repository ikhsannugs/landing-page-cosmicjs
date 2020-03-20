env.DOCKER_REGISTRY = 'ikhsannugs'
env.DOCKER_IMAGE_NAME = 'node-cosmicjs'
node('master') {
    stage('Git Pull') {
          checkout scm
    }
      stage('Build Docker Image') {
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
      stage('Push Docker Image to Dockerhub') {
          sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
          stage('Deploy to Server') {
          sh 'sed -i "s/BUILD_NUMBER/$BUILD_NUMBER/g" cosmic.yaml'    
          sh "kubectl apply -f cosmic.yaml"
    }
    
}

