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
            sh 'docker login -u $user1 -p $pass1 docker.io'
            withCredentials([usernamePassword(credentialsId: 'aw1234', passwordVariable: 'pass1', usernameVariable: 'user1')])
             {
                  sh 'echo $pass1 | sudo -kS docker tag ankush56/nodeapp:${DOCKER_TAG} ankushwalia/nodeapp:${DOCKER_TAG}'
                  sh 'echo $pass1 | sudo -kS docker push ankushwalia/nodeapp:${DOCKER_TAG}'
             }
          }
          }
        }
        stage('Deploy to K8') {
          steps {
            echo "Deploying to K8s now"
            withCredentials([usernamePassword(credentialsId: 'aw1234', passwordVariable: 'pass1', usernameVariable: 'user1')])
             {
                  sh 'echo $pwd'
                  sh 'echo $pass1 | sudo -kS chmod 775 ~/repos/nodeapp/changeTag.sh'
                  sh './~/repos/nodeapp/changeTag.sh ${DOCKER_TAG}'
                  sh """
                  sshpass -p $pass1 scp -P 22 ~/repos/nodeapp/services.yaml ~/repos/nodeapp/nodeapp.yaml aw@192.168.1.102:~/
                  sshpass -p $pass1 ssh $user1@192.168.1.102 '
                  script {
                    try {
                        microk8s kubectl apply -f .
                    }
                    catch {
                       microk8s kubectl create -f .
                    }
                  }
                  '
              """

             }
          }
        }
      }
}


def getDockertag() {
  def tag = sh script: 'git rev-parse HEAD', returnStdout: true
  return tag
}
