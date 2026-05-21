pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION    = 'us-west-2'

        DOCKER_IMAGE = "amitbaviskar/3tier-app"
        IMAGE_TAG    = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Amitbaviskar/3-tier-architecture.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init -upgrade -no-color'
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'terraform fmt -check'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate -no-color'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -no-color'
            }
        }

        stage('Terraform Apply') {
            steps {
                input message: 'Approve Terraform Apply?'

                sh 'terraform apply -auto-approve -no-color'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$IMAGE_TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$IMAGE_TAG'
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check console output.'
        }
    }
}
