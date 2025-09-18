pipeline {
    agent { label 'agent2' }

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
                sh ' docker build -t $DOCKER_IMAGE .'

            }

        }
        stage('push docker image ') {
            steps {

                sh 'docker tag $DOCKER_IMAGE   $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER'
                sh ''' 
                    echo hello
                    withCredentials([usernamePassword(credentialsId: 'docker-cred' , usernameVariable: 'USER', passwordVariable: 'PASS')])
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER
                    
                '''
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
