pipeline {
    tools {
        snyk "${SNYK_TOOL}"
        dockerTool "${DOCKER_TOOL}"
    }
    agent {
        docker {
            image 'python'
            args "--privileged"
        }
    }
    stages {
        stage('Clone repo') {
            steps {
                git "${REPO}"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'python -m pip install pipenv'
                sh 'pip3 install -r requirements.txt'
                snykSecurity failOnIssues: false, snykInstallation: "${SNYK_TOOL}", snykTokenId: "${SNYK_TOKEN_ID}", targetFile: 'requirements.txt'
                withCredentials([usernamePassword(credentialsId: "${ARTIFACTORY_TOKEN_ID}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'chmod 666 $JENKINS_HOME/jobs/jobs/${JOB_NAME}/builds/${BUILD_NUMBER}/archive/*.html && \
                    curl -u${USERNAME}:${PASSWORD} -T $JENKINS_HOME/jobs/jobs/${JOB_NAME}/builds/${BUILD_NUMBER}/archive/*.html "${ARTIFACTORY_URL}/artifactory/example-repo-local/${JOB_NAME}/snyk_report_${BUILD_NUMBER}.html"'
                }
            }
        }
    }
}