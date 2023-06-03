pipeline {
    agent {label 'smart-village'}

    stages {
      
        stage('build') {            
            steps {
                echo 'build'
                script {

                  withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                            sh '''
                               docker login -u ${USERNAME} -p ${PASSWORD}
                                  docker build -t  abdo23/bakehouseiti:v${BUILD_NUMBER} .                              
                                   docker push  abdo23/bakehouseiti:v${BUILD_NUMBER}    

                                                              

                                '''
                          }
                     }

                    
                            
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                  script {

                               withCredentials([file(credentialsId: 'file-iti-credentials', variable: 'KUBECONFIG_file')]) {
                                sh '''
                                         echo 'hello'

                                    kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_file} 
                               '''

                          }
                  }
              }
        }
    }
}
