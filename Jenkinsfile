pipeline {
    agent {label 'smart-village'}

    stages {
      
        stage('build') {            
            steps {
                echo 'build'
                script {
                  withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME_iti', passwordVariable: 'PASSWORD_iti')]) {
                            sh '''
                            docker login -u ${USERNAME_iti} -p ${PASSWORD_iti} --tty
                                docker login -u ${USERNAME} -p ${PASSWORD}
                                docker build -t  abdo23/bakehouseiti:v1 .
                              
                                docker push abdo23/bakehouseiti:v1
                                
                                '''
                       }
                }            
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                 script {
                   
                        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                            sh '''
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -f Deployment/deploy.yaml.tmp
                                kubectl apply -f Deployment --kubeconfig ${KUBECONFIG} 
                            '''
           
            }
        }
    }
}
