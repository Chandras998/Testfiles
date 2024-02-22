pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    stages {
        stage('Set Env Variables and Write to File') {
            steps {
                script {
                    // Initialize variables
                    def ENV_MAIN = ''
                    def APP = ''
                    
                    // Use simple if-else structure to set environment variables
                    if (params.ENV == 'DEV') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S-test'
                    } else if (params.ENV == 'PROD') {
                        ENV_MAIN = 'PROD'
                        APP = 'CSK8S-PRD'
                    }

                    // Write environment variables to a temporary file
                    writeFile file: "${env.WORKSPACE}/env_vars.tmp", text: "export ENV_MAIN=${ENV_MAIN}\nexport APP=${APP}"
                }
            }
        }
        
        stage('Echo Variables in Docker Container') {
            steps {
                    // Source the temporary file to set environment variables in the Docker container
                    sh '''
                        source $WORKSPACE/env_vars.tmp
                        echo "Inside Docker - ENV_MAIN: $ENV_MAIN, APP: $APP"
                    '''
                
            }
        }
    }
    post {
        always {
            // Cleanup the workspace, which includes the temporary file
            cleanWs()
        }
    }
}
