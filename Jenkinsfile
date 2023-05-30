pipeline {
    agent {label 'smart-village'}

    stages {
      
        stage('build') {            
            steps {
                echo 'build'
                script {
                  withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME_iti', passwordVariable: 'PASSWORD_iti')]) {
                            sh '''
                            docker login -u ${USERNAME_iti} -p ${PASSWORD_iti}
                                docker login -u ${USERNAME} -p ${PASSWORD}
                                docker build -t  abdo23/bakehouseiti:v${BUILD_NUMBER} .
                              
                                docker push 
                                
                                '''
                       }
                }            
            }
        }
        stage('deploy') {
            steps {
                sh " echo 'hello from jenkins lab2 '"
            }
        }
    }
}
