pipeline {
    agent any

    stages {
        stage('Check build settings') {
            steps {
                script {
                    GERRIT_REPO_URL="ssh://${env.GERRIT_HOST}/${env.GERRIT_PROJECT}"
                    TARGET_DIR="${env.GERRIT_PROJECT}"
                    git(url: GERRIT_REPO_URL, branch:"master") 
                    echo "${GERRIT_REPO_URL}"
                    sh "ls -al"
                }
            }
        }
    }
}
