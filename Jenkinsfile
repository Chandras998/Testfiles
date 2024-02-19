pipeline {
    agent any

    parameters {
        base64File name: 'FirstFile', description: 'Upload the first file here', encoding: 'BASE64'
        base64File name: 'SecondFile', description: 'Upload the second file here', encoding: 'BASE64'
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    // Check if the 'FirstFile' was uploaded
                    if (params.FirstFile) {
                        // Decoding the Base64 content of the first file
                        def firstFileContent = new String(Base64.decoder.decode(params.FirstFile), "UTF-8")
                        echo "First file content: ${firstFileContent}"
                    } else {
                        echo "First file not uploaded or not found."
                    }
                    
                    // Check if the 'SecondFile' was uploaded
                    if (params.SecondFile) {
                        // Decoding the Base64 content of the second file
                        def secondFileContent = new String(Base64.decoder.decode(params.SecondFile), "UTF-8")
                        echo "Second file content: ${secondFileContent}"
                    } else {
                        echo "Second file not uploaded or not found."
                    }
                }
            }
        }

        stage('Process Files') {
            steps {
                echo 'This stage would process the uploaded files.'
                // Add your processing steps here
            }
        }

        stage('Cleanup') {
            steps {
                echo 'This stage would clean up any resources used.'
                // Add your cleanup steps here
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
