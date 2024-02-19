pipeline {
    agent any
    parameters {
        base64File(name: 'pdfile', description: 'pdf')
    }
    stages {
      stage('rename file') {
	    steps {
            withFileParameter(name:'pdfile', allowNoFile: false) {
              sh 'mv $pdfile ${WORKSPACE}/example.pdf'
            }
        } 
      }
    }
}
