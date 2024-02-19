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
                    // Jenkins stores uploaded files in a temporary location accessible via environment variables
                    // The names of the variables are FILE_PARAMETER_NAME and FILE_PARAMETER_NAME_FILE
                    def firstFilePath = env.FirstFile
                    def secondFilePath = env.SecondFile

                    if (firstFilePath) {
                        sh "cp '${firstFilePath}' ${WORKSPACE}/firstFile.txt"
                        echo "First file uploaded successfully."
                    } else {
                        echo "First file not found or not provided."
                    }

                    if (secondFilePath) {
                        sh "cp '${secondFilePath}' ${WORKSPACE}/secondFile.txt"
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
                // Example: List files in the workspace to confirm they're there
                sh 'ls -lah'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up workspace...'
                sh 'rm -f ${WORKSPACE}/firstFile.txt ${WORKSPACE}/secondFile.txt'
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
