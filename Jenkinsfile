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
                    // Directly use the file names as uploaded; no need to prepend with WORKSPACE or env variable
                    def firstFileExists = fileExists 'FirstFile'
                    def secondFileExists = fileExists 'SecondFile'

                    if (firstFileExists) {
                        echo "First file uploaded successfully."
                        // If specific processing on the file is needed, do it here
                    } else {
                        echo "First file not found or not provided."
                    }

                    if (secondFileExists) {
                        echo "Second file uploaded successfully."
                        // If specific processing on the file is needed, do it here
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
                sh 'rm -f FirstFile SecondFile'
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
