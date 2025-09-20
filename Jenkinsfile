pipeline {
    agent any

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
                      docker build -t iti:$BUILD_NUMBER .
                      docker tag iti:$BUILD_NUMBER  abdo23/iti:$BUILD_NUMBER
                '''
                
            }
        }
        stage('deploy') {
            steps { 
                sh '''
                      docker rm -f node-app || true
                      docker run -d --name node-app -p 3000:3000  iti:$BUILD_NUMBER
                '''
                
            }
        }
    }
}
