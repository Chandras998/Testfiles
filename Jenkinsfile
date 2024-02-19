pipeline {
    agent any
    parameters {
        // Assuming stashedFile is a custom or plugin-specific parameter type
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
    }
    stages {
        stage('Prepare') {
            steps {
                // Unstash the first CSV file and move it
                unstash 'csvfile1'
                sh 'mv csvfile1 ${WORKSPACE}/chandra1.csv'
                // Unstash the second CSV file and move it
                unstash 'csvfile2'
                sh 'mv csvfile2 ${WORKSPACE}/chandra2.csv'
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
