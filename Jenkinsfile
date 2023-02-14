import groovy.json.JsonSlurper

node {
    stage('Checkout') {
        checkout scmGit(branches: [[name: '*/main']], browser: github('https://github.com/mrunalgosar/gh-auto-labeler.git'), extensions: [], userRemoteConfigs: [[credentialsId: '2bd6bfe8-2f4c-4b8f-9549-5f3d4e2496f1', url: 'https://github.com/mrunalgosar/gh-auto-labeler.git']])
        echo("Getting labels from PR branch. Branch name: ${env.BRANCH_NAME}")
        if (env.BRANCH_NAME ==~ /PR-\d+/) {
            for (label in getLabels(${env.CHANGE_ID})) {
                println("PR # ${env.CHANGE_ID} Label --> ${label}")
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
            for(label in getLabels(prNum)) {
                println("PR # ${prNum} Label --> ${label}")
            }
        }
    }
    stage('Get last merged pull request') {
        echo("Get last merged pull request number via GH Api")
        if (env.BRANCH_NAME == 'main') {
            def prNum = getPrNum()
            def list = getLabels(prNum)
            for (label in list) {
                println("PR # ${prNum} Label --> ${label}")
            }
        }
    }
}

def getPrNum() {
    def prNum = 0;
    withCredentials([usernamePassword(credentialsId: 'postman', passwordVariable: 'PPP', usernameVariable: 'UUU')]) {
        def response = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: false, customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${PPP}"]], responseHandle: 'LEAVE_OPEN', url: "https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls?state=closed&per_page=1&page=1", wrapAsMultipart: false
        def json = new JsonSlurper().parseText(response.content)
        println("PR #: ${json[0].number}")
        prNum = json[0].number
        response.close()
    }
    return prNum
}

def getLabels(prNum) {
    def list = new ArrayList();
    withCredentials([usernamePassword(credentialsId: 'postman', passwordVariable: 'PPP', usernameVariable: 'UUU')]) {
        def response = httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: false, customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${PPP}"]], responseHandle: 'LEAVE_OPEN', url: "https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls/${prNum}", wrapAsMultipart: false
        def json = new JsonSlurper().parseText(response.content)
        for (label in json.labels) {
            echo("Label: ${label.name}")
            list.add(label.name)
        }
        response.close()
    }
    return list;
}