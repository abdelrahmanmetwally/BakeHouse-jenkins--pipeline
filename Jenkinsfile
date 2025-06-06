pipeline {
    agent {
        label "smart-village"
    }
    parameters {
        choice(name: 'BRANCH_NAME', choices: ['dev', 'test', 'prod', "release"])
    }
    stages {
        stage('build') {
            steps {
                script {
                    echo 'build'
                    if (BRANCH_NAME == "dev" || BRANCH_NAME == "test" || BRANCH_NAME == "prod") {
                        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                            sh '''
                                docker login -u ${USERNAME} -p ${PASSWORD}
                                docker build -t abdo23/bakehouseiti:v${BUILD_NUMBER} .
                                docker push abdo23/bakehouseiti:v${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build_num.txt
                                echo ${BRANCH_NAME}
                            '''
                        }
                    } else {
                        echo "user chose ${BRANCH_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                    if (BRANCH_NAME == "release") {
                        withCredentials([file(credentialsId: 'file-iti-credentials', variable: 'KUBECONFIG_ITI')]) {
                            sh '''
                                export BUILD_NUMBER=$(cat ../build_num.txt)
                                export release=$(helm list --short | grep ^bakehouse)
                                if [ -z $release ]
                                then 
                                     helm install bakehouse ./bakehouse --values bakehouse/$(BRANCH_NAME)-values.yaml\
                                     --set build  BUILD_NUMBER=${BUILD_NUMBER}
                                else
                                     helm upgrade bakehouse ./bakehouse --values bakehouse/$(BRANCH_NAME)-values.yaml\
                                     --set build  BUILD_NUMBER=${BUILD_NUMBER}
                                fi     
                               
                               '''
                        }
                    } else {
                        echo "user chose ${BRANCH_NAME}"
                    }
                }
            }
        }
    }
}
