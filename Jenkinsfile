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
                sh(script: '''
                    #!/bin/bash
                    ENV_MAIN="" # Initialize the variable
                    APP=""

                    if [ "${ENV}" = "DEV" ]; then
                        ENV_MAIN="NONPROD"
                        APP="CSK8S"
                    elif [ "${ENV}" = "QA" ]; then
                        ENV_MAIN="NONPROD"
                        APP="CSK8S-qa"
                    elif [ "${ENV}" = "TEST" ]; then
                        ENV_MAIN="NONPROD"
                        APP="CSK8S-test"
                    elif [ "${ENV}" = "PROD" ]; then
                        ENV_MAIN="PROD"
                        APP="CSK8S-PRD"
                    fi

                    # Echo the values
                    echo "ENV_MAIN is ${ENV_MAIN} and APP is ${APP}"
                    ''', returnStdout: false)
                
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
