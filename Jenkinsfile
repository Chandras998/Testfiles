pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'])
    }
    stages {
        stage('List workspace contents') {
            steps {
                sh 'ls -lart'
            }
        }
        stage('Prepare') {
            steps {
                unstash 'csvfile1'
                sh 'touch csvfile1'
                unstash 'csvfile2'
                sh 'touch csvfile2'
            }
        }
        stage('Test') {
            steps {
                sh "ls -al ${WORKSPACE}/"
                sh "sleep 10s"
            }
        }
        stage('Test Env') {
            steps {
                script {
                    def ENV_MAIN = '' // Initialize the variable
                    def APP = ''

                    if (params.ENV == 'DEV') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S-NP'
                    } else if (params.ENV == 'PROD') {
                        ENV_MAIN = 'PROD'
                        APP = 'CSK8S-PRD'
                    }

                    // Echo the values
                    echo "ENV_MAIN is ${ENV_MAIN} and APP is ${APP}"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
