pipeline {
    agent { label 'agent2' }

    stages {
        stage('run pipeline') {
            steps {
       //         git 'https://github.com/abdelrahmanmetwally/BakeHouse-jenkins--pipeline.git'
                echo ' start from github repo '
                sh ' docker build -t first-try .'
                sh 'docker tag first-try   abdo23/first-try:$BUILD_NUMBER'
                sh ''' 
                    echo hello
                    echo Abdo@01287306586 | docker login -u abdo23 --password-stdin
                    docker push abdo23/first-try:$BUILD_NUMBER
                 
                    docker rm -f nginx-container  || true
                    docker run -d --name nginx-container -p 80:80 abdo23/first-try:$BUILD_NUMBER
                '''
            }
            post {
                success {
                    echo "push succeeded ya man"
                }
                failure {
                    echo "failed ya man"
                }
            }
        }
    }
}
