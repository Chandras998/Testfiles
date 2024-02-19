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
                    def firstFileExists = fileExists('firstFile')
                    def secondFileExists = fileExists('secondFile')

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
                    if (fileExists('firstFile') && fileExists('secondFile')) {
                        echo 'Both files are present. Processing now...'
                        // Example: sh 'cat firstFile'
                        // Example: sh 'cat secondFile'
                    } else {
                        echo 'One or both files are missing; skipping processing.'
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up workspace...'
                // Example: sh 'rm -f firstFile secondFile'
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
