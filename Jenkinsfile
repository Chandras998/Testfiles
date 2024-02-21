pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'])
    }
    stages {
        stage('List workspace contents') {
            steps {
                sh 'ls -lart'
            }
        }
        stage('Prepare') {
            steps {
                // Unstash the first CSV file
                unstash 'csvfile1'
                // Refresh the timestamp of the file
                sh 'touch csvfile1'

                // Unstash the second CSV file
                unstash 'csvfile2'
                // Refresh the timestamp of the file
                sh 'touch csvfile2'
            }
        }
        stage('Test') {
            steps {
                // List files in the workspace to verify
                sh "ls -al ${WORKSPACE}/"
                // Sleep for 10 seconds
                sh "sleep 10s"
            }
        }
        stage('Test Env') {
            steps{
                script {
                    // Corrected shell script for conditional logic
                    def shellCommand = """
                    if [ "\${params.ENV}" = "DEV" ]; then
                        ENV_MAIN='NONPROD'
                        APP='CSK8S'
                    elif [ "\${params.ENV}" = "QA" ]; then
                        ENV_MAIN='NONPROD'
                        APP='CSK8S-qa'
                    elif [ "\${params.ENV}" = "TEST" ]; then
                        ENV_MAIN='NONPROD'
                        APP='CSK8S-NP'
                    elif [ "\${params.ENV}" = "PROD" ]; then
                        ENV_MAIN='PROD'
                        APP='CSK8S-PRD'
                    fi
                    echo ENV_MAIN=\$ENV_MAIN
                    echo APP=\$APP
                    """
                    
                    // Execute the shell script
                    sh(script: shellCommand, returnStdout: false)
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
