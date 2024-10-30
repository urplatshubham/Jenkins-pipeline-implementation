# Jenkins CI/CD Pipeline for Flask Application

This repository demonstrates a Jenkins CI/CD pipeline for a Flask application. The pipeline is configured to run on a Windows environment and automates code checkout, dependency installation, testing, and deployment.

## Overview

The Jenkins pipeline performs the following stages:
1. **Checkout Code**: Confirms the code is checked out by Jenkins.
2. **Build**: Sets up a virtual environment and installs dependencies from `requirements.txt`.
3. **Test**: Runs unit tests using `pytest`.
4. **Deploy**: Deploys to a staging environment (simulated) if the pipeline is triggered on the `main` branch.

### Jenkinsfile

The `Jenkinsfile` contains the pipeline configuration, including environment setup, stages, and the necessary commands for Windows.

### Prerequisites

- **Jenkins** installed and configured on a Windows server.
- **Git**, **Python**, and **pip** installed on the Jenkins server.
- **Python packages** listed in `requirements.txt`, including Flask and pytest.

## Pipeline Setup

1. **Clone this repository**:
   ```bash
   git clone https://github.com/urplatshubham/Jenkins-pipeline-implementation
   ```

2. **Configure Jenkins Job**:
   - Create a new pipeline job in Jenkins.
   - Set **Pipeline script from SCM** and provide the repository URL.
   - Specify the `main` branch if necessary.

3. **Push Changes**:
   - Any changes pushed to the `main` branch will automatically trigger the pipeline.

## Jenkinsfile Content

Here’s the content of the `Jenkinsfile` used for this pipeline:

```groovy
pipeline {
    agent any

    environment {
        VENV_DIR = "venv" // Directory for virtual environment
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Code has already been checked out by Jenkins'
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

    // Optional: Comment out or remove the post section to avoid email errors if not configured
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
```

### Explanation of Stages
- **Checkout Code**: Jenkins checks out the code by default; this stage simply confirms it.
- **Build**: Sets up a Python virtual environment and installs required packages.
- **Test**: Activates the environment and runs tests with `pytest`.
- **Deploy**: Outputs a success message to simulate deployment when triggered on `main`.

## Notes
- **Email Notifications**: Email notifications are optional and currently commented out to avoid SMTP setup errors.
- **Compatibility**: This pipeline is specifically configured for Jenkins running on Windows.

## Troubleshooting
If the pipeline fails:
1. Check **Console Output** in Jenkins for error details.
2. Ensure **Python** and **Git** are installed and accessible in Jenkins.
3. Verify that **requirements.txt** includes all necessary dependencies.
