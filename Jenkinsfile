pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'])
    }
    environment {
        // Initialize as empty, will set dynamically
        ENV_MAIN = '' 
        APP = ''
    }
    stages {
        // Previous stages unchanged

        // Dynamically set environment variables based on parameter
        stage('Set Env Variables') {
            steps {
                script {
                    // Set ENV_MAIN and APP based on ENV parameter
                    if (params.ENV == 'DEV') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S-test'
                    } else if (params.ENV == 'PROD') {
                        env.ENV_MAIN = 'PROD'
                        env.APP = 'CSK8S-PRD'
                    }
                }
            }
        }

        // New stage to echo variables within docker-in-docker container
        stage('Echo Variables in Docker') {
            steps {
                container('docker-in-docker') {
                    // Now echoing Jenkins environment variables
                    sh '''
                        echo "Echoing variables within docker-in-docker container..."
                        echo "ENV_MAIN: ${ENV_MAIN}"
                        echo "APP: ${APP}"
                    '''
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
