pipeline {
    agent any

    stages {

        stage('Test') {
            steps {
                echo 'Testing..'
                snykSecurity failOnIssues: false, snykInstallation: "${SNYK_TOOL}", snykTokenId: "${SNYK_TOKEN_ID}"
                withCredentials([usernamePassword(credentialsId: "${ARTIFACTORY_TOKEN_ID}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'curl -u${USERNAME}:${PASSWORD} -T $JENKINS_HOME/jobs/jobs/${JOB_NAME}/builds/${BUILD_NUMBER}/archive/*.html "${ARTIFACTORY_URL}/artifactory/example-repo-local/${JOB_NAME}/snyk_report_${BUILD_NUMBER}.html"'
                }
            }
        }
    }
}