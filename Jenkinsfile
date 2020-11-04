pipeline {
    agent any

    stages {
        stage('Checkout GPVB Repository') {
            steps {
                script {
                    GERRIT_REPO_URL="ssh://${env.GERRIT_HOST}/${env.GERRIT_PROJECT}"
                    TARGET_DIR="${env.GERRIT_PROJECT}"
                    git(url: GERRIT_REPO_URL, branch:"master") 
                    echo "${GERRIT_REPO_URL}"

                    sh "git checkout master && git remote update --prune"
                    sh "git branch -D review || echo 'review branch doesn't exist"
                    sh "git fetch origin ${env.GERRIT_REFSPEC}:review && git checkout review"
                }
            }
        }
        stage('Check Build_starfish_* parameters') {
            steps {
                script {
                    def data = readFile(file: 'build-settings.bash')
                    String[] lines
                    lines = data.split("\n")
                    echo lines[0]
                }
            }
        }
    }
}
