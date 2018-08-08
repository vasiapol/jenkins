#!groovy

node{
    stage('SCM Checkout'){
        git 'https://github.com/vasiapol/hello-world-war.git'
    }
    stage ('Mvn package'){
        sh 'mvn clean package'
    }
    stage ('Deploy tomcat'){
        sh 'sshpass -p \'55555' scp -P 22 target/*.war root@192.168.103.220:/var/lib/tomcat/webapps/'
    }
 }


