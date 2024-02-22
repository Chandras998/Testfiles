pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'Upload your first CSV file.')
        stashedFile(name: 'csvfile2', description: 'Upload your second CSV file.')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    stages {
        stage('Set Env Variables and Echo') {
            steps {
                script {
                    // Directly set and echo variables without using env.ENV_MAIN
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

                    // Echo variables within the script block for immediate feedback
                    echo "ENV_MAIN: ${ENV_MAIN}, APP: ${APP}"
                }
            }
        }
        stage('Echo Variables Separately in Docker') {
            steps {
                    // Use script block to echo variables; ensure they are passed as parameters to the container
                    script {
                        def mainEnv = env.ENV_MAIN ?: 'Not Set'
                        def app = env.APP ?: 'Not Set'
                        sh "echo 'Inside Docker - ENV_MAIN: ${mainEnv}, APP: ${app}'"
                    
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
