pipeline {
    agent any

    parameters {
        file(name: 'firstFile', description: 'Upload the first file here')
        file(name: 'secondFile', description: 'Upload the second file here')
    }

    stages {
        stage('Check Files') {
            steps {
                script {
                    // Initialize variables to check if files exist
                    def firstFileExists = fileExists('firstFile')
                    def secondFileExists = fileExists('secondFile')

                    // Echo based on whether each file exists
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
                    // Processing uploaded files, example shown with echo
                    if (fileExists('firstFile') && fileExists('secondFile')) {
                        echo 'Both files are present. Processing now...'
                        // Placeholder for actual processing commands
                        // For example, you might want to move, rename, or analyze the files
                    } else {
                        echo 'One or both files are missing; skipping processing.'
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Cleanup actions, for example, removing files after processing
                    if (fileExists('firstFile')) {
                        sh 'rm -f firstFile'
                        echo 'First file removed.'
                    }
                    if (fileExists('secondFile')) {
                        sh 'rm -f secondFile'
                        echo 'Second file removed.'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
