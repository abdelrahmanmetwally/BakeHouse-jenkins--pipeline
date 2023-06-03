pipeline {
    agent {label 'smart-village'}

    stages {
      
        stage('build') {            
            steps {
                echo 'build'
                script {
//                   if (BRANCH_NAME == "dev" || BRANCH_NAME == "test" || BRANCH_NAME == "preprod") {
                  withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                            sh '''
                               docker login -u ${USERNAME} -p ${PASSWORD}
//                                 ${BUILD_NUMBER} . 
                                 sudo docker build -t  abdo23/bakehouseiti:v1 .                              
                                sudo docker push  abdo23/bakehouseiti:v1      
//                                echo ${BUILD_NUMBER} > ../build.txt
                                                              

                                '''
                          }
                     }
//                         else {  echo "thanks"}
                    
                            
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
//                  script {
//                            if (BRANCH_NAME == "release") {
//                         withCredentials([file(credentialsId: 'file-iti-credentials', variable: 'KUBECONFIG_file')]) {
//                             sh '''
                                         echo 'hello'
//                                 export ${BUILD_NUMBER} = $(cat ../build.txt)
//                                 mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
//                                 cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
//                                 rm -f Deployment/deploy.yaml.tmp
//                                 kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_file} -n ${BRANCH_NAME}
//                             '''
//                          }
//                         }
//                 }
              }
        }
    }
}
