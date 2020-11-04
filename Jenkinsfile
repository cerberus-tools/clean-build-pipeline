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

                    def build_layer_pattern = ~/Build_starfish_.*="(true|false)"/
                    def selected_build_layers = []
                    for ( String line_str: lines) {
                        def build_layer_matcher = line_str =~ build_layer_pattern
                        if ( build_layer_matcher.find() ) {
                            def splitted_line = line_str.split("=")
                            def layer_name = splitted_line[0]
                            def build_on_off = splitted_line[1]
                            if ( build_on_off == "\"true\"" ) {
                                selected_build_layers.add("Trigger ${layer_name}")
                            }
                        }
                    } 
                    def trigger_message = selected_build_layers.join('\n')
                    sh "echo \"{'message': '${trigger_message}'}\"| ssh ${env.GERRIT_HOST} gerrit review ${env.GERRIT_PATCHSET_REVISION} -j"
                }
            }
        }
    }
}
