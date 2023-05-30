pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                sh "ls" 
            }
        }
        stage('build') {
            steps {
                sh '''
                echo ${BUILD_NUMBER}
                '''
            }
        }
        stage('deploy') {
            steps {
                sh " echo 'hello from jenkins lab2 '"
            }
        }
    }
}
