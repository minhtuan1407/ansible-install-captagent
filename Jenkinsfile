pipeline {
    agent { label 'built-in' }
    options {
        ansiColor('vga')
    }
    parameters {
        string(name: 'ip_server', defaultValue: '', description: '')
        choice(name: 'sip_portrange', choices: ['50061-50062', '5060-5061'], description: '')
        string(name: 'homer_server', defaultValue: '103.127.206.135', description: '')
    }
    stages {
        stage ("Install captagent") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/ansible-install-captagent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'install-captagent',
                    extraVars: [
                        ip_server: [value: '${ip_server}', hidden: false],
                        sip_portrange: [value: '${sip_portrange}', hidden: false],
                        homer_server: [value: '${homer_server}', hidden: false]
                    ]
                )
            }
        }
        stage ("Copy config file") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/ansible-install-captagent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'copy-config-file',
                    extraVars: [
                        ip_server: [value: '${ip_server}', hidden: false],
                        sip_portrange: [value: '${sip_portrange}', hidden: false],
                        homer_server: [value: '${homer_server}', hidden: false]
                    ]
                )
            }
        }
        stage ("Restart captagent") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/ansible-install-captagent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'restart-captagent',
                    extraVars: [
                        ip_server: [value: '${ip_server}', hidden: false],
                        sip_portrange: [value: '${sip_portrange}', hidden: false],
                        homer_server: [value: '${homer_server}', hidden: false]
                    ]
                )
            }
        }
    }
    post {
        always {
            // notifyEvents message: '''Tuan is testing
            // Build <a href="$PROJECT_URL">$PROJECT_NAME</a>
            // Build Number <a href="$BUILD_URL">$BUILD_NUMBER</a> result with status: <b>$BUILD_STATUS</b>
            // <a href="$BUILD_URL/console">Build log</a> on host ${ip_server}''',
            //     token: 'zqMYpf7aCt0Wl3T_IMdsh-LOUzf7_G8T'

            // emailext subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME}',
            // body: '''Build <a href="$PROJECT_URL">$PROJECT_NAME</a> <br>
            // Build Number <a href="$BUILD_URL">$BUILD_NUMBER</a> result with status: <b>$BUILD_STATUS</b> <br>
            // <a href="$BUILD_URL/console">Build log</a> on host ${ip_server}''',
            // to: 'tech@tel4vn.com',
            // attachLog: true

            telegramSend(message: '''Build [$PROJECT_NAME]($PROJECT_URL) \nBuild Number [$BUILD_NUMBER]($BUILD_URL) result with status: *$BUILD_STATUS* \n[Build log]($BUILD_URL/console) on host ${ip_server}''',
            chatId:1284662325)
        }
    }
}
