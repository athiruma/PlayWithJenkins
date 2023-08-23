def accountList = ['appeng', 'test']
pipeline {
    options {
        disableConcurrentBuilds()
    }
    agent {
        docker {
            label 'cloud-governance-worker'
            image 'quay.io/athiru/centos-stream8-podman:latest'
            args  '-u root -v /etc/postfix/main.cf:/etc/postfix/main.cf --privileged'
        }
    }
    environment {
        LDAP_HOST_NAME = credentials('cloud-governance-ldap-host-name')
        account_name = "coreos-training"
        contact1 = "ebattat@redhat.com"
        contact2 = "athiruma@redhat.com"
    }
    stages {
        stage('Checkout') { // Checkout (git clone ...) the projects repository
           steps {
                 checkout scm
           }
        }
        stage('Iterate the loop'){
            steps{
                script {
                    for (def account in accountList) {
                        env.account = credentials('${account.toUpperCase()}')
                        python3 test.py
                        
                    }
                }
            }
        }
    }
}
