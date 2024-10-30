pipeline {
    agent any

    environment {
        VENV_DIR = "venv" // Directory for virtual environment
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                sh 'git clone https://github.com/urplatshubham/Jenkins-pipeline-implementation .'
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh '''
                    python3 -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    . ${VENV_DIR}/bin/activate
                    pytest
                '''
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to staging environment...'
                // Simulate deployment steps
                sh 'echo "Deployment successful"'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
            mail to: 'you@example.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Good news! The job ${env.JOB_NAME} has successfully completed."
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
            mail to: 'you@example.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Oops! The job ${env.JOB_NAME} has failed. Check the Jenkins logs for details."
        }
    }
}
