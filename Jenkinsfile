pipeline {
    agent any
    parameters {
        // Assuming stashedFile is a custom or plugin-specific parameter type
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
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
    }
}
