pipeline {
    agent {
        label 'any'
    }
    parameters {
        stashedFile description: 'pdf', name: 'pdfile'
    }
    stages {
        stage('prepare') {
            steps {
                unstash 'pdfile'
                sh 'mv pdfile ${WORKSPACE}/example.pdf'
            }
        }
        stage('test') {
            steps {
                sh "ls -al ${WORKSPACE}/"
                sh "sleep 10s"
            }
        }
    }
}
