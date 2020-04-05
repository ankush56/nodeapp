pipeline
{
    environment {
      DOCKER_TAG = getDockertag()
    }
    agent any
    stages
      {
        stage ('Build Docker image')
        {
          steps
          {
            echo "Building Docker image"
            withCredentials([usernamePassword(credentialsId: 'aw1234', passwordVariable: 'pass1', usernameVariable: 'user1')])
             {
                sh 'echo $pass1 | sudo -kS docker build -t ankush56/nodeapp:${DOCKER_TAG} .'
             }
          }
        }
        stage ('Docker hub push')
        {
          steps
          {
            echo "Pushing to Docker hub"
            withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass1', usernameVariable: 'user1')]) {
            sh 'docker login -u $user1 -p $pass1' docker.io
            withCredentials([usernamePassword(credentialsId: 'aw1234', passwordVariable: 'pass1', usernameVariable: 'user1')])
             {
                  sh 'echo $pass1 | sudo -kS docker tag ankush56/nodeapp:${DOCKER_TAG} ankushwalia/nodeapp:${DOCKER_TAG}'
                  sh 'echo $pass1 | sudo -kS docker push ankush56/nodeapp:${DOCKER_TAG}'
             }
          }
          }
        }
      }
}


def getDockertag() {
  def tag = sh script: 'git rev-parse HEAD', returnStdout: true
  return tag
}
