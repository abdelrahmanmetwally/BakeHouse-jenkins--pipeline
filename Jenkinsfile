pipeline {
    agent any
    environment {
        DOCKER_CONTAINER = 'node-app'
        DOCKER_IMAGE = 'iti'

    }
    options {
        timeout(time: 1, unit: 'HOURS') 
    }


    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                
            }
        }
        stage('docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'USER' , passwordVariable: 'PASS')]) {
                  sh '''
                     echo $PASS | docker login -u $USER --password-stdin
                     
                  '''
                }
            }
        }
        stage('build and tagging ') {
            steps {
                sh '''
                      docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .
                      docker tag $DOCKER_IMAGE:$BUILD_NUMBER  abdo23/$DOCKER_IMAGE:$BUILD_NUMBER
                '''
                
            }
        }
        stage('deploy') {
            steps { 
                sh '''
                      docker rm -f $DOCKER_CONTAINER || true
                      docker run -d --name $DOCKER_CONTAINER -p 3000:3000  $DOCKER_IMAGE:$BUILD_NUMBER
                '''
                
            }
            
        }
    }
    post {
        success {
            echo "pipeline succeeded "
            build job: 'freestyle-1'
   }
        failure {
            echo "pipeline failed"
          }
    }
}
