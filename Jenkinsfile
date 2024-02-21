pipeline {
    agent any
    parameters {
        // Assuming stashedFile is a custom or plugin-specific parameter type
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
                sh (script: '''
                if "$ENV" = "DEV"; then
                    ENV_MAIN='NONPROD'
                    APP='CSK8S'
                    echo "$ENV_MAIN"
                elif "$ENV" = "QA"; then
                    ENV_MAIN='NONPROD'
                    APP='CSK8S-qa'
                elif "$ENV" = "TEST"; then
                    ENV_MAIN='NONPROD'
                    APP='CSK8S-NP'
                elif "$ENV" = "PROD"; then
                    ENV_MAIN='NONPROD'
                    APP='CSK8S-PRD'
                fi
                echo "$ENV_MAIN"
                ''', returnStdout: false)
                
            
            }
        }
    }
}
