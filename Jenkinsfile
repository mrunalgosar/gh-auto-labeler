import groovy.json.JsonSlurper

node {
    stage('Checkout') {
        checkout scmGit(branches: [[name: '*/main']], browser: github('https://github.com/mrunalgosar/gh-auto-labeler.git'), extensions: [], userRemoteConfigs: [[credentialsId: '2bd6bfe8-2f4c-4b8f-9549-5f3d4e2496f1', url: 'https://github.com/mrunalgosar/gh-auto-labeler.git']])
        withCredentials([string(credentialsId: 'postman', variable: 'TOKEN')]) {
            def response = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: false, customHeaders: [[maskValue: true, name: 'Authorization', value: 'Bearer ${TOKEN}']], responseHandle: 'NONE', url: 'https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls/2', wrapAsMultipart: false
            def json = new JsonSlurper().parseText(response.content)
            for (label in json.labels) {
                echo("Label: ${label.name}")
            }
        }
    }
}