pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'Upload your first CSV file.')
        stashedFile(name: 'csvfile2', description: 'Upload your second CSV file.')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    environment {
        // Placeholder for dynamic environment variable assignment
        ENV_MAIN = ''
        APP = ''
    }
    stages {
        stage('Set Env Variables') {
            steps {
                script {
                    // Logic for setting environment variables based on the "ENV" parameter
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
        stage('Echo Variables Separately') {
            steps {
                container('docker-in-docker') {
                    // Use a Groovy string to construct the shell command with environment variables
                    script {
                        def cmd = "echo 'Echoing variables within docker-in-docker container...';" +
                                  "echo 'ENV_MAIN: ${env.ENV_MAIN}';" +
                                  "echo 'APP: ${env.APP}';"
                        sh script: cmd, label: 'Show ENV_MAIN and APP'
                    }
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
