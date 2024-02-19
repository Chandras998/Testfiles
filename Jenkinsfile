pipeline {
    agent any

    parameters {
        file(name: 'firstFile', description: 'Upload the first file')
        file(name: 'secondFile', description: 'Upload the second file')
    }

    stages {
        stage('Check Files') {
            steps {
                script {
                    // Check if files exist in the workspace
                    def firstFileExists = fileExists 'firstFile'
                    def secondFileExists = fileExists 'secondFile'

                    if (firstFileExists) {
                        echo "First file is uploaded and available in the workspace."
                    } else {
                        echo "First file not found or not provided."
                    }

                    if (secondFileExists) {
                        echo "Second file is uploaded and available in the workspace."
                    } else {
                        echo "Second file not found or not provided."
                    }
                }
            }
        }

        stage('Process Files') {
            steps {
                script {
                    // Placeholder for processing uploaded files
                    // Example: sh 'ls -l'
                }
            }
        }

        // Add more stages as needed for your pipeline
    }
}
