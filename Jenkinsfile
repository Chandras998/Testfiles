pipeline {
    agent any
    parameters {
        file(name: 'firstFile', description: 'Upload the first file here')
        file(name: 'secondFile', description: 'Upload the second file here')
    }
    stages {
        stage('Check Upload') {
            steps {
                script {
                    if (fileExists('firstFile')) {
                        echo 'First file is present.'
                    } else {
                        echo 'First file is missing.'
                    }
                    if (fileExists('secondFile')) {
                        echo 'Second file is present.'
                    } else {
                        echo 'Second file is missing.'
                    }
                }
            }
        }
    }
}
