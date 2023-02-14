import groovy.json.JsonSlurper

node {
    stage('Checkout') {
        checkout scmGit(branches: [[name: '*/main']], browser: github('https://github.com/mrunalgosar/gh-auto-labeler.git'), extensions: [], userRemoteConfigs: [[credentialsId: '2bd6bfe8-2f4c-4b8f-9549-5f3d4e2496f1', url: 'https://github.com/mrunalgosar/gh-auto-labeler.git']])
        echo("Getting labels from PR branch. Branch name: ${env.BRANCH_NAME}")
        if (env.BRANCH_NAME ==~ /PR-\d+/) {
            withCredentials([usernamePassword(credentialsId: 'postman', passwordVariable: 'PPP', usernameVariable: 'UUU')]) {
                def response = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: false, customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${PPP}"]], responseHandle: 'LEAVE_OPEN', url: "https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls/${env.CHANGE_ID}", wrapAsMultipart: false
                def json = new JsonSlurper().parseText(response.content)
                for (label in json.labels) {
                    echo("Label: ${label.name}")
                }
                response.close()
            }
        }
    }
    stage('Get Head') {
        echo("Getting last merged pull request # without squash & merge. Branch name: ${env.BRANCH_NAME}")
        if (env.BRANCH_NAME == 'main') {
            println("Merge requests")
            sh("git log --grep=\"Merge pull request\" --pretty=oneline")
            println("Merge requests #")
            def prNum = sh returnStdout: true, script: 'git log --grep="Merge pull request" --pretty=oneline -1 | sed -En \'s/.*#([[:digit:]]+).*/\\1/p\''
            printf("PR #: %s", prNum)
            withCredentials([usernamePassword(credentialsId: 'postman', passwordVariable: 'PPP', usernameVariable: 'UUU')]) {
                def response = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: false, customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${PPP}"]], responseHandle: 'LEAVE_OPEN', url: "https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls/${prNum}", wrapAsMultipart: false
                def json = new JsonSlurper().parseText(response.content)
                for (label in json.labels) {
                    echo("Label: ${label.name}")
                }
                response.close()
            }
        }
    }
    stage('Get last merged pull request') {
        echo("Get last merged pull request number via GH Api")
        if (env.BRANCH_NAME == 'main') {
            withCredentials([usernamePassword(credentialsId: 'postman', passwordVariable: 'PPP', usernameVariable: 'UUU')]) {
                def response = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: false, customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${PPP}"]], responseHandle: 'LEAVE_OPEN', url: "https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls?state=closed&per_page=1&page=1", wrapAsMultipart: false
                def json = new JsonSlurper().parseText(response.content)
                println("PR #: ${json[0].number}")
                def prNum = json[0].number
                response.close()
                def res = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: true, customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${PPP}"]], responseHandle: 'LEAVE_OPEN', url: "https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls/${prNum}", wrapAsMultipart: false
                println(res.status)
                println("Response ============ \n ${res.content}")
                json = new JsonSlurper().parseText(res.content)
                for (label in json.labels) {
                    echo("Label: ${label.name}")
                }
                res.close()
            }
        }
    }
}