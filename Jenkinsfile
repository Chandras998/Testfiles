pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    stages {
        stage('Set Env Variables and Write to File') {
            steps {
                script {
                    // Determine environment variables based on the parameter
                    ENV_MAIN = (params.ENV == 'DEV' || params.ENV == 'QA' || params.ENV == 'TEST') ? 'NONPROD' : 'PROD'
                    APP = (params.ENV == 'DEV') ? 'CSK8S' : (params.ENV == 'QA') ? 'CSK8S-qa' : (params.ENV == 'TEST') ? 'CSK8S-test' : 'CSK8S-PRD'
                    
                    // Write environment variables to a temporary file in the workspace
                    writeFile file: "${env.WORKSPACE}/env_vars.tmp", text: "ENV_MAIN=${ENV_MAIN}\nAPP=${APP}"
                }
            }
        }
        
        stage('Echo Variables in Docker Container') {
            steps {
                    // Read variables from the file using the absolute path and echo them
                    sh '''
                        source $WORKSPACE/env_vars.tmp
                        echo "Inside Docker - ENV_MAIN: $ENV_MAIN, APP: $APP"
                    '''
                
            }
        }
    }
    post {
        always {
            // Cleanup the temporary file along with the workspace
            cleanWs()
        }
    }
}
