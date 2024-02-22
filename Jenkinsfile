pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'Upload your first CSV file.')
        stashedFile(name: 'csvfile2', description: 'Upload your second CSV file.')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    stages {
        // Previous setup and stages unchanged...

        stage('Set Env Variables') {
            steps {
                script {
                    // Logic to set ENV_MAIN and APP based on ENV parameter
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
                
                    // Directly inject the values into the shell command
                    sh """
                        ENV_MAIN='${env.ENV_MAIN}' APP='${env.APP}' bash -c 'echo "Echoing variables separately..."; echo "ENV_MAIN: \$ENV_MAIN"; echo "APP: \$APP";'
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
