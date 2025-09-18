pipeline {
    agent { label 'agent1' }
  
    options {
        timestamps()
        timeout(time: 20 , unit: 'MINUTES')
    }

    environment {
        CONTAINER_NAME = 'nginx-app'  
        DOCKER_REGISTRY = 'abdo23'
        DOCKER_IMAGE = 'first-try'
        DEPLOY_ENV = 'dev'
       // MY_CREDS = credentials('docker-cred')   > used with>  echo  $MY_CREDS_PSW | docker login -u $MY_CREDS_USR --password-stdin

    }


    stages {
        stage('build docker image') {
            steps {
                // or use username and password (token) 
                git 'https://github.com/abdelrahmanmetwally/BakeHouse-jenkins--pipeline.git'
                echo ' start '
                sh ' docker build -f ./other-dockerfile/Dockerfile -t $DOCKER_IMAGE ./other-Dockerfile/'

            }

        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker tag $DOCKER_IMAGE $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER"

                    withCredentials([usernamePassword( credentialsId: 'docker-cred', usernameVariable: 'USER', passwordVariable: 'PASS' )]) {
                    sh """
                        echo $PASS | docker login -u $USER --password-stdin 
                        docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER
                    """
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                sh ' docker rm -f $CONTAINER_NAME-$DEPLOY_ENV  || true'
                sh 'docker run -d --name $CONTAINER_NAME-$DEPLOY_ENV -p 80:80 $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER'
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
