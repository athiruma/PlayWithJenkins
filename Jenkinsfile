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
                        def access_key = credentials("${account}-aws-access-key-id")
                        def secret_key = credentials("${account}-aws-secret-key-id")
                        def s3_bucket = credentials("${account}-s3-bucket")
                        def account_name = "${account}"
                        echo "Account Name: $account_name"

                        withCredentials([string(credentialsId: 'appeng-aws-access-key-id', variable: 'access_key'),
                                         string(credentials: 'appeng-aws-secret-key-id', variable: 'secret_key'),
                                         string(credentialsId: 'appeng-s3-bucket', variable: 's3_bucket'),
                                         string(credentialsId: 'appeng-s3-bucket', variable: 's3_bucket')
                                        ]) {
                            echo "$s3_bucket"
                            
                        }
                    }
                }
            }
        }
    }
}
