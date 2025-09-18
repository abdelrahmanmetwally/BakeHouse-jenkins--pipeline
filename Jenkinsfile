
pipeline {
    agent { label 'agent2' }

    option {
        timestamps()
        timeout(time: 20 , unit: 'MINUTES')
    }

    environment {
        CONTAINER_NAME = 'nginx-app'
        DEPLOY_ENV = 'dev'
        MY_CREDS = credentials('docker-cred')
    }

    stages {
        stage('run pipeline') {
            steps {
       //         git 'https://github.com/abdelrahmanmetwally/BakeHouse-jenkins--pipeline.git'
                echo ' start from github repo '
                sh ' docker build -t first-try .'
                sh 'docker tag first-try   abdo23/first-try:$BUILD_NUMBER'
                sh ''' 
                    echo hello
                    // withCredentials([usernamePassword(credentialsId: 'docker-cred' , usernameVariable: 'USER', passwordVariable: 'PASS')])
                    // echo $PASS | docker login -u $USER --password-stdin
                    echo  $MY_CREDS_PSW | docker login -u $MY_CREDS_USR --password-stdin
                    docker push abdo23/first-try:$BUILD_NUMBER
                 
                    docker rm -f $CONTAINER_NAME-$DEPLOY_ENV  || true
                    docker run -d --name $CONTAINER_NAME-$DEPLOY_ENV -p 80:80 abdo23/first-try:$BUILD_NUMBER
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


























