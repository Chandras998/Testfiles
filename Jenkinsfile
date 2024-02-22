pipeline {
    agent any
    parameters {
        stashedFile(name: 'csvfile1', description: 'First CSV file upload')
        stashedFile(name: 'csvfile2', description: 'Second CSV file upload')
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'])
    }
    environment {
        ENV_MAIN = '' 
        APP = ''
    }
    stages {
        stage('List workspace contents') {
            steps {
                sh 'ls -lart'
            }
        }
        stage('Prepare') {
            steps {
                unstash 'csvfile1'
                sh 'touch csvfile1'
                unstash 'csvfile2'
                sh 'touch csvfile2'
            }
        }
        stage('Test') {
            steps {
                sh "ls -al ${WORKSPACE}/"
                sh "sleep 3s"
            }
        }
        stage('Set Env Variables') {
            steps {
                script {
                    // Dynamically set ENV_MAIN and APP based on the selected ENV
                    if (params.ENV == 'DEV') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        env.ENV_MAIN = 'NONPROD'
                        env.APP = 'CSK8S-test'
                    } else if (params.ENV == 'PROD') {
                        env.ENV_MAIN = 'PROD'
                        env.APP = 'CSK8S-PRD'
                    }
                }
            }
        }
        stage('Echo Variables in Docker') {
            steps {
                
                    // Echoing ENV_MAIN and APP within the docker-in-docker container
                    script {
                        // Passing Jenkins environment variables to shell script
                        def envMain = env.ENV_MAIN
                        def app = env.APP
                        
                        sh """
                           echo "Echoing variables within docker-in-docker container..."
                           echo "ENV_MAIN: $envMain"
                           echo "APP: $app"
                        """
                    
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
