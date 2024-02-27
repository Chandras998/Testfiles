pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    stages {
        stage('Set Env Variables and Write to File') {
            steps {
                script {
                    def ENV_MAIN = ''
                    def APP = ''
                    
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

                    writeFile file: "${env.WORKSPACE}/env_vars.tmp", text: "ENV_MAIN=${ENV_MAIN}\nAPP=${APP}"
                }
            }
        }

        stage('Trigger Downstream Job') {
            steps {
                script {
                    def props = readProperties file: "${env.WORKSPACE}/env_vars.tmp"
                    echo "Read from properties file: ENV_MAIN=${props['ENV_MAIN']}, APP=${props['APP']}"

                    if (!props['ENV_MAIN'] || !props['APP']) {
                        echo "ENV_MAIN or APP is missing or empty in properties file."
                        error("Stopping the build due to missing parameter values.")
                    }

                    // Note: Changed from hard-coded strings to actual values from properties file
                    build job: 'main-deployment', parameters: [
                        string(name: 'ENV_MAIN', value: props['ENV_MAIN'].trim()), 
                        string(name: 'APP', value: props['APP'].trim())
                    ], wait: false
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
