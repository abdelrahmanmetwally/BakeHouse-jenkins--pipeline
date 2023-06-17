pipeline {
    agent { label 'smart-village' }
    stages {
        stage('build') {
            steps {
                echo 'build'
                script{
                        
                                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                                    sh '''
                                        docker login -u ${USERNAME} -p ${PASSWORD}
                                        docker build -t abdo23/bakehouseiti:v${BUILD_NUMBER} .
                                        docker push abdo23/bakehouseiti:v${BUILD_NUMBER}
                                        echo ${BUILD_NUMBER} > ../build.txt
                                    '''
                                }
                    
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                         
                                withCredentials([file(credentialsId: 'file-iti-credentials', variable: 'KUBECONFIG')]) {
                                    sh '''
                                        export BUILD_NUMBER=$(cat ../build.txt)
                                        mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                        cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                        rm -f Deployment/deploy.yaml.tmp
                                        kubectl apply -f Deployment --kubeconfig ${KUBECONFIG} 
                                    '''
                                }
                         
                    }
                }
          }
    }
}
