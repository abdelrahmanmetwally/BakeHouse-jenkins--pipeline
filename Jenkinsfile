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
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -rf Deployment/deploy.yaml.tmp
                                kubectl apply -f Deployment --kubeconfig ${KUBECONFIG_ITI} -n ${BRANCH_NAME}
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
