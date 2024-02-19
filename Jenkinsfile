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
                    // Adjusting the check to handle null values and ensuring files are provided
                    if (params.FirstFile != null && params.FirstFile.toString() != '') {
                        // Assuming params.FirstFile is the name of the file for simplicity
                        def firstFileName = params.FirstFile.getOriginalFileName()
                        sh "cp '${firstFileName}' ${WORKSPACE}/"
                        echo "First file uploaded successfully."
                    } else {
                        echo "First file not found or not provided."
                    }

                    if (params.SecondFile != null && params.SecondFile.toString() != '') {
                        // Assuming params.SecondFile is the name of the file for simplicity
                        def secondFileName = params.SecondFile.getOriginalFileName()
                        sh "cp '${secondFileName}' ${WORKSPACE}/"
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
                // Enhanced cleanup step to dynamically remove uploaded files if they exist
                script {
                    if (params.FirstFile != null && params.FirstFile.toString() != '') {
                        def firstFileName = params.FirstFile.getOriginalFileName()
                        sh "rm -f ${WORKSPACE}/${firstFileName}"
                    }

                    if (params.SecondFile != null && params.SecondFile.toString() != '') {
                        def secondFileName = params.SecondFile.getOriginalFileName()
                        sh "rm -f ${WORKSPACE}/${secondFileName}"
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
