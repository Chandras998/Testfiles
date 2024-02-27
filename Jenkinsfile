pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
    }
    stages {
        stage('Set Env Variables and Write to File') {
            steps {
                script {
                    def ENV_MAIN = ''
                    def APP = ''
                    
                    if (params.ENV == 'DEV') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S'
                    } else if (params.ENV == 'QA') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S-qa'
                    } else if (params.ENV == 'TEST') {
                        ENV_MAIN = 'NONPROD'
                        APP = 'CSK8S-test'
                    } else if (params.ENV == 'PROD') {
                        ENV_MAIN = 'PROD'
                        APP = 'CSK8S-PRD'
                    }

                    writeFile file: "${env.WORKSPACE}/env_vars.tmp", text: "ENV_MAIN=${ENV_MAIN}\nAPP=${APP}"
                }
            }
        }

        stage('Trigger Downstream Job') {
            steps {
                script {
                    def props = readProperties file: "${env.WORKSPACE}/env_vars.tmp"
                    if (props['ENV_MAIN'] && props['APP']) {
                        build job: 'main-deployment', parameters: [
                            string(name: 'ENV_MAIN', value: props['ENV_MAIN']),
                            string(name: 'APP', value: props['APP'])
                        ], wait: false
                    } else {
                        echo "Required parameters are missing."
                    }
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
