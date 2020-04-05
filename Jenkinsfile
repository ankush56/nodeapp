pipeline
{
    environment {
      DOCKER_TAG = getDockertag()
    }
    agent any
    stages
      {
        stage ('Build DOcker image')
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
      }
}


def getDockertag() {
  def tag = sh script: 'git rev-parse HEAD', returnStdout: true
  return tag
}
