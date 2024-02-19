pipeline {
    agent any
    parameters {
        // Assuming stashedFile is a custom or plugin-specific parameter type; otherwise, this might not work as expected.
        // Replace with 'file' if you're uploading a file directly.
        stashedFile(name: 'pdfile', description: 'pdf')
    }
    stages {
        stage('Prepare') {
            steps {
                // Assuming 'pdfile' is a stash name; unstash it
                unstash 'pdfile'
                // Moving the unstashed file, assuming it's named 'pdfile' in the workspace
                sh 'mv pdfile ${WORKSPACE}/example.pdf'
            }
        }
        stage('Test') {
            steps {
                // List files in the workspace
                sh "ls -al ${WORKSPACE}/"
                // Sleep for 10 seconds
                sh "sleep 10s"
            }
        }
    }
}
