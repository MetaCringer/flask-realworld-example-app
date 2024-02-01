pipeline {
    tools {
        snyk "${SNYK_TOOL}"
        dockerTool "${DOCKER_TOOL}"
    }
    agent {
        docker {
            image 'python'
            args "-u root"
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
                snykSecurity failOnIssues: false, snykInstallation: "${SNYK_TOOL}", snykTokenId: "${SNYK_TOKEN_ID}"
                withCredentials([usernamePassword(credentialsId: "${ARTIFACTORY_TOKEN_ID}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'curl -u${USERNAME}:${PASSWORD} -T $JENKINS_HOME/jobs/jobs/${JOB_NAME}/builds/${BUILD_NUMBER}/archive/*.html "${ARTIFACTORY_URL}/artifactory/example-repo-local/${JOB_NAME}/snyk_report_${BUILD_NUMBER}.html"'
                }
            }
        }
    }
}