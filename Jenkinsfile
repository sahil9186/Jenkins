pipeline {
    agent any

    environment {
        DEV_SERVER = "172.31.33.250"
        STG_SERVER = "172.31.39.220"
        PRD_SERVER = "172.31.43.126"
        USER = "root"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Apache') {
            steps {
                sh '''
                sudo yum install httpd -y
                sudo systemctl start httpd
                sudo systemctl enable httpd
                '''
            }
        }

        stage('Deploy to DEV') {
            when {
                branch 'dev'
            }
            steps {
                sh '''
                sudo cp index.html /var/www/html/index.html
                sudo systemctl restart httpd
                '''
            }
        }

        stage('Deploy to STG') {
            when {
                branch 'stg'
            }
            steps {
                sh '''
                sudo cp index.html /var/www/html/index.html
                sudo systemctl restart httpd
                '''
            }
        }

        stage('Deploy to PROD') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                sudo cp index.html /var/www/html/index.html
                sudo systemctl restart httpd
                '''
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
