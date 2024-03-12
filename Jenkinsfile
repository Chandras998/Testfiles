pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['DEV', 'QA', 'TEST', 'PROD'], description: 'Select the environment.')
        booleanParam(name: 'CHECK', defaultValue: false, description: '')
    }
    environment {
        myremoteuser = "adminuser"
        myremote_host = "server1.com"
        mongohostname = "servermongo.com"
        mongousername = "nodeuser"
        mongopassword = "welcome123"
        MY_DIR = "/tmp/css"
        ssh_options = "-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ConnectTimeout=10 -o ServerAliveInterval=10 -o ServerAliveCountMax=3 -o TCPKeepAlive=yes"
    }
    stages {
        
        stage('Buildstage') {
            steps {
                container('kubectl') {
                    sh '''
                        #!/bin/bash
                        set -xe
                        
                        DATE=$(date +%Y%m%d)

                        if [ ! -z "$ENDDATE" ] && [[ $ENDDATE =~ ^[0-9]{4}-[0-9]{2}-[0-9]{2}$ ]]; then
                            ENDDATE_NEW=$ENDDATE
                        elif [ ! -z "$ENDDATE" ]; then
                            echo "wrong date format"
                            exit 1
                        else 
                            ENDDATE_NEW=$(date -d @$(( $(date +%s) - 86400 )) +%Y-%m-%d)
                        fi

                        if [ ! -z "$STARTDATE" ] && [[ $STARTDATE =~ ^[0-9]{4}-[0-9]{2}-[0-9]{2}$ ]]; then
                            STARTDATE_NEW=$STARTDATE
                        elif [ ! -z "$STARTDATE" ]; then
                            echo "wrong date format"
                            exit 1
                        else 
                            STARTDATE_NEW=$(date -d @$(( $(date +%s) - 86400 * 30 )) +%Y-%m-%d)
                        fi

                        echo "Enddate is: $ENDDATE_NEW"
                        echo "Startdate is: $STARTDATE_NEW"

                        ssh -t $ssh_options $myremoteuser@$myremote_host "mongoexport --host=$mongohostname --username=$mongousername --password=$mongopassword --authenticationDatabase=msql_auth --db my-cp-db --collection req_log -q='{\"requestDateTime\": {\"\$gte\": {\"\$date\": \"${STARTDATE_NEW}T00:00:00.00Z\"}, \"\$lte\": {\"\$date\": \"${ENDDATE_NEW}T00:00:00.00Z\"}}}' --type=csv --fields _id,reqDatetime,Identifier,SSsystem,_class -out=$MY_DIR/mydelgreport_${DATE}.csv"
                    '''
                }
            }
        }
    }
}
