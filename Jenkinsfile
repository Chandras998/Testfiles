pipeline {
    agent any
    
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
                sh '''
                    #!/bin/bash
                    set -xe
                    
                    DATE=$(date +%Y%m%d)
                    ENDDATE_NEW=$(date -d @$(( $(date +%s) - 86400 )) +%Y-%m-%d)
                    STARTDATE_NEW=$(date -d @$(( $(date +%s) - 86400 * 30 )) +%Y-%m-%d)
                    
                    echo "Enddate is: $ENDDATE_NEW"
                    echo "Startdate is: $STARTDATE_NEW"

                    ssh $ssh_options $myremoteuser@$myremote_host "mongoexport --host=$mongohostname --username=$mongousername --password=$mongopassword --authenticationDatabase=msql_auth --db my-cp-db --collection req_log -q '{\"requestDateTime\": {\"\$gte\": {\"\$date\": \"${STARTDATE_NEW}T00:00:00.00Z\"}, \"\$lte\": {\"\$date\": \"${ENDDATE_NEW}T00:00:00.00Z\"}}}' --type=csv --fields _id,reqDatetime,Identifier,SSsystem,_class -out=$MY_DIR/mydelgreport_${DATE}.csv"
                '''
            }
        }
    }
}
