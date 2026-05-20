pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AKIA2Y2622OQC5FGRDOI')
        AWS_SECRET_ACCESS_KEY = credentials('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')
        AWS_DEFAULT_REGION    = 'us-west-2'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Amitbaviskar/3-tier-architecture.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'terraform fmt -check'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }

        stage('Terraform Apply') {
            steps {
                input message: 'Do you want to apply Terraform changes?'
                sh 'terraform apply -auto-approve'
            }
        }
    }
}
