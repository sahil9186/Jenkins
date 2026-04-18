pipeline {
    agent any

    environment {
        DEV_SERVER = "3.110.178.21"
        STG_SERVER = "3.111.144.226"
        PRD_SERVER = "13.232.160.131"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to DEV') {
            when {
                branch 'dev'
            }
            steps {
                sshagent(['ec2-ssh']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no index.html ec2-user@$DEV_SERVER:/tmp/
                    ssh ec2-user@$DEV_SERVER "sudo cp /tmp/index.html /var/www/html/index.html && sudo systemctl restart httpd"
                    '''
                }
            }
        }

        stage('Deploy to STG') {
            when {
                branch 'stg'
            }
            steps {
                sshagent(['ec2-ssh']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no index.html ec2-user@$STG_SERVER:/tmp/
                    ssh ec2-user@$STG_SERVER "sudo cp /tmp/index.html /var/www/html/index.html && sudo systemctl restart httpd"
                    '''
                }
            }
        }

        stage('Deploy to PROD') {
            when {
                branch 'main'
            }
            steps {
                sshagent(['ec2-ssh']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no index.html ec2-user@$PRD_SERVER:/tmp/
                    ssh ec2-user@$PRD_SERVER "sudo cp /tmp/index.html /var/www/html/index.html && sudo systemctl restart httpd"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful"
        }
        failure {
            echo "Deployment Failed"
        }
    }
}
