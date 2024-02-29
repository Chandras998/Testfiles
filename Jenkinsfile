pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
        booleanParam(name: 'CHECK', defaultValue: false, description: '')
    }
    stages {
        stage('Set Env Variables and Write to File') {
            steps {
                script {
                    def ENV_MAIN = ''
                    def APP = ''
                    
                    if (params.ENV == 'DEV') {
                        ENV_MAIN = 'NONPROD'
                    } else if (params.ENV == 'QA') {
                        ENV_MAIN = 'NONPROD'
                    } else if (params.ENV == 'TEST') {
                        ENV_MAIN = 'NONPROD'
                    } else if (params.ENV == 'PROD') {
                        ENV_MAIN = 'PROD'
                    }
                    
                    APP = 'CSK8S' // Assuming APP is 'CSK8S' for simplification

                    writeFile file: "${env.WORKSPACE}/env_vars.tmp", text: "ENV_MAIN=${ENV_MAIN}\nAPP=${APP}"
                }
            }
        }
        stage('Intentional Failure') {
            when {
                expression { !params.CHECK } // Only fail if CHECK is not true
            }
            steps {
                script {
                    error("Intentionally failing the build to test downstream job conditions.")
                }
            }
        }

        stage('Trigger Downstream Job') {
            steps {
                script {
                    sh "ls -lart"
                    sh "cat ${env.WORKSPACE}/env_vars.tmp"
                    def props = readProperties file: "${env.WORKSPACE}/env_vars.tmp"
                    
                    echo "Read from properties file: ENV_MAIN=${props['ENV_MAIN']}, APP=${props['APP']}"

                    if (!props['ENV_MAIN'] || !props['APP']) {
                        echo "ENV_MAIN or APP is missing or empty in properties file."
                        error("Stopping the build due to missing parameter values.")
                    }

                    // Debugging output before triggering the job
                    echo "Triggering with ENV_MAIN: '${props['ENV_MAIN']}', APP: '${props['APP']}'"
                    if ((currentBuild.result == null || currentBuild.result == 'SUCCESS' || currentBuild.result == 'UNSTABLE') || params.CHECK) {
                        build job: 'main-deployment', parameters: [
                            string(name: 'ENV_MAIN', value: props['ENV_MAIN'].toString().trim()),
                            string(name: 'APP', value: props['APP'].toString().trim())
                        ], wait: false
                    } else {
                        echo "Skipping main-deployment due to current build status or CHECK parameter."
                    }
                }
            }
        }
        stage('Second Downstream Job') {
            steps {
                script {
                    sh "ls -lart"
                    sh "cat ${env.WORKSPACE}/env_vars.tmp"
                    def props = readProperties file: "${env.WORKSPACE}/env_vars.tmp"
                    
                    echo "Read from properties file: ENV_MAIN=${props['ENV_MAIN']}, APP=${props['APP']}"

                    if (!props['ENV_MAIN'] || !props['APP']) {
                        echo "ENV_MAIN or APP is missing or empty in properties file."
                        error("Stopping the build due to missing parameter values.")
                    }

                    // Debugging output before triggering the job
                    echo "Triggering with ENV_MAIN: '${props['ENV_MAIN']}', APP: '${props['APP']}'"
                    if ((currentBuild.result == null || currentBuild.result == 'SUCCESS' || currentBuild.result == 'UNSTABLE') || params.CHECK) {
                        build job: 'second-deployment', parameters: [
                            string(name: 'ENV_MAIN', value: props['ENV_MAIN'].toString().trim()),
                            string(name: 'APP', value: props['APP'].toString().trim())
                        ], wait: false
                    } else {
                        echo "Skipping main-deployment due to current build status or CHECK parameter."
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
