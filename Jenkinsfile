
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
        MY_CREDS = credentials('docker-cred')
    }

    stages {
        stage('run pipeline') {
            steps {
       //         git 'https://github.com/abdelrahmanmetwally/BakeHouse-jenkins--pipeline.git'
                echo ' start from github repo '
                sh ' docker build -t first-try .'
                sh 'docker tag $DOCKER_IMAGE   $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER'
                sh ''' 
                    echo hello
                    // withCredentials([usernamePassword(credentialsId: 'docker-cred' , usernameVariable: 'USER', passwordVariable: 'PASS')])
                    // echo $PASS | docker login -u $USER --password-stdin
                    echo  $MY_CREDS_PSW | docker login -u $MY_CREDS_USR --password-stdin
                    docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER
                 
                    docker rm -f $CONTAINER_NAME-$DEPLOY_ENV  || true
                    docker run -d --name $CONTAINER_NAME-$DEPLOY_ENV -p 80:80 $DOCKER_REGISTRY/$DOCKER_IMAGE:$BUILD_NUMBER
                '''
            }
            post {
                success {
                    echo "push succeeded ya man"
                    build job: 'freestyle-1'
                }
                failure {
                    echo "failed ya man"
                }
            }
        }
    }
}


























