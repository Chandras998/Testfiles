pipeline {
    agent any // or specify 'label' if you need to run on a specific node

    parameters {
        base64File name: 'firstFile', description: 'Upload the first file'
        base64File name: 'secondFile', description: 'Upload the second file'
    }

    stages {
        stage('Prepare and Rename First File') {
            steps {
                withFileParameter('firstFile') {
                    // This block will only execute if 'firstFile' is uploaded.
                    sh 'mv "$firstFile" "renamedFirstFile.ext"' // Replace '.ext' with the actual file extension
                    echo 'First file renamed to renamedFirstFile.ext'
                }
            }
        }
        
        stage('Prepare and Rename Second File') {
            steps {
                withFileParameter('secondFile') {
                    // This block will only execute if 'secondFile' is uploaded.
                    sh 'mv "$secondFile" "renamedSecondFile.ext"' // Replace '.ext' with the actual file extension
                    echo 'Second file renamed to renamedSecondFile.ext'
                }
            }
        }
        
        // Add additional stages if you need to process the files
        stage('Process Files') {
            steps {
                echo 'Here you would add steps to process the renamed files.'
                // Example: sh 'process-script.sh renamedFirstFile.ext'
                // Example: sh 'process-script.sh renamedSecondFile.ext'
            }
        }
        
        // Add a cleanup stage if necessary
        stage('Cleanup') {
            steps {
                echo 'Cleaning up workspace...'
                sh 'rm -f renamedFirstFile.ext renamedSecondFile.ext' // Replace '.ext' with the actual file extension
            }
        }
    }
    
    post {
        always {
            echo 'This will always run after the stages, regardless of the result.'
        }
    }
}
