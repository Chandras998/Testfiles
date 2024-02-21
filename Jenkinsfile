pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'])
    }
    environment {
        ENV_MAIN = '' // Define the variable at the pipeline level if needed
    }
    stages {
        stage('List workspace contents') {
            steps {
                sh 'ls -lart'
            }
        }
        stage('Prepare') {
            steps {
                // Unstash the first CSV file
                unstash 'csvfile1'
                // Refresh the timestamp of the file
                sh 'touch csvfile1'

                // Unstash the second CSV file
                unstash 'csvfile2'
                // Refresh the timestamp of the file
                sh 'touch csvfile2'
            }
        }
        stage('Test') {
            steps {
                // List files in the workspace to verify
                sh "ls -al ${WORKSPACE}/"
                // Sleep for 10 seconds
                sh "sleep 10s"
            }
        }
        stage('Test Env') {
            steps {
                script {
                    // Use Groovy to set ENV_MAIN and APP based on the ENV parameter
                    if (params.ENV == 'DEV') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S-NP'
                    } else if (params.ENV == 'PROD') {
                        env.ENV_MAIN = 'PROD' // Fixed to correctly reflect PROD environment
                        env.APP = 'CSK8S-PRD'
                    }
                }
                // Echo the values to confirm
                sh "echo ENV_MAIN is ${env.ENV_MAIN} and APP is ${env.APP}"
            }
        }
    }
}
