pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'Upload your first CSV file.')
        stashedFile(name: 'csvfile2', description: 'Upload your second CSV file.')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    environment {
        // These are placeholders and will be set dynamically
        ENV_MAIN = ''
        APP = ''
    }
    stages {
        stage('Set Env Variables and Echo') {
            steps {
                script {
                    // Dynamically set the environment variables
                    def localEnvMain = ''
                    def localApp = ''
                    if (params.ENV == 'DEV') {
                        localEnvMain = 'NONPROD'
                        localApp = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        localEnvMain = 'NONPROD'
                        localApp = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        localEnvMain = 'NONPROD'
                        localApp = 'CSK8S-test'
                    } else if (params.ENV == 'PROD') {
                        localEnvMain = 'PROD'
                        localApp = 'CSK8S-PRD'
                    }
                    // Set Jenkins environment variables for use in later stages
                    env.ENV_MAIN = localEnvMain
                    env.APP = localApp
                    // Echo the variables for immediate feedback
                    echo "Set ENV_MAIN: $localEnvMain, APP: $localApp"
                }
            }
        }
        stage('Echo Variables Separately in Docker') {
            steps {
                    // Inject the environment variables into the Docker command
                    sh """
                        echo 'Inside Docker - ENV_MAIN: \${ENV_MAIN}, APP: \${APP}'
                    """
                
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
