pipeline {
    agent any
    environment {
        // Define environment variable to store AWS secret access key
        AWS_SECRET_ACCESS_KEY = credentials('secret_access_key')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sayalisa2li7/terraform-project'
            }
        }

        stage('Terraform Init') {
            steps {
                // Use withCredentials block to securely provide AWS secret access key
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: '',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                    credentialsId: 'secret_access_key'
                ]]) {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }

        stage('Delay before Destroy') {
            steps {
                script {
                    echo 'Waiting for 5 minutes before destroying resources...'
                    sleep time: 300, unit: 'SECONDS'  // Add a delay of 5 minutes (300 seconds)
                }
            }
        }

        stage('Terraform Destroy') {
            steps {
                sh 'terraform destroy -auto-approve'
            }
        }
    }
}
