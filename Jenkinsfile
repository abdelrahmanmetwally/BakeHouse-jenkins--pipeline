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
                    if (BRANCH_NAME == "release") {
                        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME_SYSADMIN', passwordVariable: 'PASSWORD_SYSADMIN')]) {
                            sh '''
                                docker login -u ${USERNAME_SYSADMIN} -p ${PASSWORD_SYSADMIN}
                                docker build -t abdo23/bakehouseiti:v${BUILD_NUMBER} .
                                docker push abdo23/bakehouseiti:v${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build_num.txt
                                echo ${ENV_ITI}
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
                    if (BRANCH_NAME == "dev" || BRANCH_NAME == "test" || BRANCH_NAME == "prod") {
                        withCredentials([file(credentialsId: 'file-iti-credentials', variable: 'KUBECONFIG_ITI')]) {
                            sh '''
                                export BUILD_NUMBER=$(cat ../build_num.txt)
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -rf Deployment/deploy.yaml.tmp
                                kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_ITI} 
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
