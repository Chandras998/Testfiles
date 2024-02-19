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
                    // The files are uploaded to the root of the workspace directory
                    // Use the 'fileExists' step to check if the files exist
                    def firstFileExists = fileExists 'FirstFile'
                    def secondFileExists = fileExists 'SecondFile'

                    if (firstFileExists) {
                        echo "First file uploaded successfully."
                    } else {
                        echo "First file not found or not provided."
                    }

                    if (secondFileExists) {
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
                // If the files exist, they can be processed here
                sh 'ls -lah'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up workspace...'
                // Clean up the workspace if necessary
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
