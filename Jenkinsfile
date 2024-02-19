pipeline {
    agent any

    parameters {
        file(name: 'firstFile', description: 'Upload the first file here')
        file(name: 'secondFile', description: 'Upload the second file here')
    }

    stages {
        stage('Verify Files') {
            steps {
                echo 'Checking for uploaded files...'
                script {
                    def files = ['firstFile', 'secondFile']
                    files.each {
                        if (fileExists(it)) {
                            echo "$it uploaded successfully."
                        } else {
                            echo "$it not found or not provided."
                        }
                    }
                }
            }
        }
    }
}
