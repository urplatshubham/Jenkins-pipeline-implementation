pipeline {
    agent any

    environment {
        VENV_DIR = "venv" // Directory for virtual environment
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                // Cloning the repository
                bat 'git clone https://github.com/urplatshubham/Jenkins-pipeline-implementation .'
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                bat """
                    python -m venv ${VENV_DIR}
                    ${VENV_DIR}\\Scripts\\activate
                    pip install -r requirements.txt
                """
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat """
                    ${VENV_DIR}\\Scripts\\activate
                    pytest
                """
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to staging environment...'
                bat 'echo "Deployment successful"'
            }
        }
    }

    // Remove or comment out the post section to avoid email errors
    /*
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
    */
}
