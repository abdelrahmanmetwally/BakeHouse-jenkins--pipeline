pipeline {
    agent any
   
    options {
        timestamps()
        timeout(time: 20 , unit: 'MINUTES')
    }
    parameters {
        string(name: 'CONTAINER_NAME', defaultValue: 'nginx-app', description: 'Base container name')
        choice(name: 'DEPLOY_ENV', choices: ['dev','staging','prod'], description: 'Deployment environment')
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
                sh ' docker build -f ./other-dockerfile/Dockerfile -t $DOCKER_IMAGE ./other-dockerfile/'

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
        stage('Approval to Deploy') {
            steps {
                timeout(time: 15, unit: 'MINUTES') {
                    input message: "Deploy ${DOCKER_IMAGE}:${BUILD_NUMBER} to ${DEPLOY_ENV}?", ok: 'Deploy'
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
