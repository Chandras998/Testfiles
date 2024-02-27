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
                    writeFile file: "${env.WORKSPACE}/env_vars.tmp", text: "ENV_MAIN=${ENV_MAIN}\nAPP=${APP}"
                }
            }
        }
        
        /*stage('Echo Variables in Docker Container') {
            steps {
                container('docker-in-docker') {
                    // Read and source the temporary file to set environment variables in the Docker container
                    sh '''
                        source $WORKSPACE/env_vars.tmp
                        echo "Inside Docker - ENV_MAIN: $ENV_MAIN, APP: $APP"
                    '''
                }
            }
        }*/

        stage('Trigger Downstream Job') {
    steps {
        script {
            // Read the properties file to get variables
            def props = readProperties file: "${env.WORKSPACE}/env_vars.tmp"
            
            // Log the properties to verify their content
            echo "Read from properties file: ENV_MAIN=${props['ENV_MAIN']}, APP=${props['APP']}"

            // Ensure that the properties are not null or empty
            if (!props['ENV_MAIN'] || !props['APP']) {
                echo "ENV_MAIN or APP is missing or empty in properties file."
                error("Stopping the build due to missing parameter values.")
            }

            // Trigger the downstream job with parameters
            build job: 'main-deployment', parameters: [
                string(name: 'ENV_MAIN', value: props['ENV_MAIN'].trim()),
                string(name: 'APP', value: props['APP'].trim())
            ], wait: false
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
