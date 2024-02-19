pipeline {
    agent any

    parameters {
        file(name: 'FirstFile', description: 'Upload the first file here')
        file(name: 'SecondFile', description: 'Upload the second file here')
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    // Check if the file parameters are provided
                    if (env.FirstFile != null) {
                        // Use the 'cp' command to copy the file from the Jenkins default location to the current workspace
                        sh "cp '${WORKSPACE}/${env.FirstFile}' ."
                        echo "First file uploaded successfully."
                    } else {
                        echo "First file not found or not provided."
                    }

                    if (env.SecondFile != null) {
                        sh "cp '${WORKSPACE}/${env.SecondFile}' ."
                        echo "Second file uploaded successfully."
                    } else {
                        echo "Second file not found or not provided."
                    }
                }
            }
        }

        stage('Process Files') {
            steps {
                echo 'Processing uploaded files...'
                // Example: List files in the workspace
                sh 'ls -lah'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up workspace...'
                // Add commands to clean up if necessary
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
