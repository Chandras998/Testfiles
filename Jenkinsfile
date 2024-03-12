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
                    set -xe
                    DATE=$(date +%Y%m%d)
                
                    if [ ! -z "$ENDDATE" ]; then
                        ENDDATE_NEW=$ENDDATE
                    else 
                        ENDDATE_NEW=$(date -d @$(( $(date +%s) - 86400 )) +%Y-%m-%d)
                    fi
                
                    if [ ! -z "$STARTDATE" ]; then
                        STARTDATE_NEW=$STARTDATE
                    else 
                        STARTDATE_NEW=$(date -d @$(( $(date +%s) - 86400 * 30 )) +%Y-%m-%d)
                    fi
                
                    echo "Enddate is: $ENDDATE_NEW"
                    echo "Startdate is: $STARTDATE_NEW"

                    QUERY=$(printf '{"requestDateTime": {"$gte": {"$date": "%sT00:00:00.00Z"}, "$lte": {"$date": "%sT00:00:00.00Z"}}}' "$STARTDATE_NEW" "$ENDDATE_NEW")
                    
                    ssh $ssh_options $myremoteuser@$myremote_host "mongoexport --host=$mongohostname --username=$mongousername --password=$mongopassword --authenticationDatabase=msql_auth --db my-cp-db --collection req_log -q '$QUERY' --type=csv --fields _id,reqDatetime,Identifier,SSsystem,_class -out=$MY_DIR/mydelgreport_$DATE.csv"
                '''
            }
        }
    }
}
