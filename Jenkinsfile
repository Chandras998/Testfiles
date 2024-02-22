pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'Upload your first CSV file.')
        stashedFile(name: 'csvfile2', description: 'Upload your second CSV file.')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    environment {
        ENV_MAIN = '' // Will be dynamically set
        APP = '' // Will be dynamically set
    }
    stages {
        stage('Set Env Variables') {
            steps {
                script {
                    // Setting environment variables based on the parameter
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
                    // Echo the variables here to verify they're set
                    println("ENV_MAIN set to: ${env.ENV_MAIN}, APP set to: ${env.APP}")
                }
            }
        }
        stage('Echo Variables Separately') {
            steps {
                
                    // Use Groovy string interpolation to construct the command
                    script {
                        // Construct the command with Groovy string interpolation
                        def cmd = """
                            echo 'Echoing variables separately...'
                            echo 'ENV_MAIN: ${env.ENV_MAIN}'
                            echo 'APP: ${env.APP}'
                        """
                        // Execute the constructed command
                        sh(cmd)
                    
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
