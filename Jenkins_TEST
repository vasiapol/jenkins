
    pipeline {

        agent any
  
        stages {
                 
            stage('Remote ssh') {
                
                steps {

                    script {

                        def remote = [:] 
                            remote.name = 'test'
                            remote.host = "${TESTHOST}"
                            remote.user = "${TESTUSER}"
                            remote.password = "${TESTPASSWORD}"
                            remote.allowAnyHosts = true
                      
                            stage('Pulling changes') {
                        
                                 sshCommand remote: remote, command: "cd API_Server_Demo;git checkout Dev; git pull origin Dev"
                       
                            } 

                            stage('Installing requirements') {
                            
                                sshCommand remote: remote, command: "cd API_Server_Demo;pip install -r requirements.txt"
                            }
                                  
                            stage('Restarting API') {
                                
                                sshCommand remote: remote, command: "systemctl restart API"
                  
                            }
        
                            stage('Runing tests') {
                
                                sshCommand remote: remote, command: "newman run API_TESTS.json"
                            }
                            
                            stage(' Archive repository') {
                
                                sshCommand remote: remote, command: "tar cvzf API_DEMO.tgz /root/API_Server_Demo"
                            }
                            
                            stage('Delivering to an Amazon S3 Bucket') {
                
                                sshCommand remote: remote, command: "aws s3 cp API_DEMO.tgz  s3://lv-335-api"
                            }
                                
                    }
                }
            }
        }

        post {

            success {  

                slackSend  color: "good", message: "`${env.JOB_NAME}` #${env.BUILD_NUMBER} was successful:"
            }
            
            failure {
                
                slackSend color: "danger", message: "`${env.JOB_NAME}` #${env.BUILD_NUMBER} was failed:"
            }
                
        }
    }

