def accountList = ['appeng']
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
        contact1 = "ebattat@redhat.com"
        contact2 = "athiruma@redhat.com"
        ES_HOST = credentials('haim-cloud-governance-elasticsearch-url')
        ES_PORT = credentials('haim-cloud-governance-elasticsearch-port')
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
                        echo "Account Name: $account_name"
                        withCredentials([string(credentialsId: "${account}-aws-access-key-id", variable: 'access_key'),
                                        string(credentialsId: "${account}-aws-secret-key-id", variable: 'secret_key'),
                                        string(credentialsId: "${account}-s3-bucket", variable: 's3_bucket')]) {
                            env.account_name = "$account"
                            sh 'python3 test.py'
                        }
                       
                    }
                }
            }
        }
    }
}
