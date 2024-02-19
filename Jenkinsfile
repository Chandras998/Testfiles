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
                    // Copy files from parameters to the workspace
                    def firstFile = params.FirstFile
                    def secondFile = params.SecondFile

                    if (firstFile != '') {
                        sh "cp '${firstFile}' ${WORKSPACE}/"
                        echo "First file uploaded successfully."
                    } else {
                        echo "First file not found."
                    }

                    if (secondFile != '') {
                        sh "cp '${secondFile}' ${WORKSPACE}/"
                        echo "Second file uploaded successfully."
                    } else {
                        echo "Second file not found."
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
                // Example cleanup step
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
