pipeline{
  agent{
    node{
      label '410977002'
    }
  }
  environment{
    DOCKERHUB_CREDENTIALS = credentials('410977002-DockerHub')
  }
  options {
      skipDefaultCheckout(true)
  }
  stages{
    stage('Clean old DOCs & chekcout SCM'){
      steps {
                cleanWs()
                checkout scm
            }
    }
    stage('verify tools') {
            steps {
                sh '''
                docker info
                docker version
                docker-compose version
                '''
            } 
        }
    stage('Login Dockerhub'){
      steps{
        sh '''
          echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
        '''
      }
    }
    stage('Build image for application'){
      steps{
        sh '''
          docker build
        '''
      }
    }
    stage('Rename && Push image to Dockerhub'){
      steps{
        sh '''
          docker tag  410977002pushdocker_ci4_service:1_1 410977002-DockerHub/410977002pushdocker_ci4_service:1_1
          docker push 410977002-DockerHub/410977002pushdocker_ci4_service:1_1
        '''
      }
    }
    stage('Start Container'){
        steps {
            sh '''
            docker-compose up -d
            '''
        }
}
    stage('Check Container'){
      steps{
        sh '''
           docker ls
        '''
      }
    }
   }
   post {
        always {
          sh '''
          docker-compose down
          docker system prune -a -f
          docker logout
          '''
        }
      }
}
