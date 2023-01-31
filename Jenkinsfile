node {
    stage('Checkout') {
        checkout scmGit(branches: [[name: '*/main']], browser: github('https://github.com/mrunalgosar/gh-auto-labeler.git'), extensions: [], userRemoteConfigs: [[credentialsId: '2bd6bfe8-2f4c-4b8f-9549-5f3d4e2496f1', url: 'https://github.com/mrunalgosar/gh-auto-labeler.git']])
        httpRequest acceptType: 'APPLICATION_JSON', consoleLogResponseBody: true, customHeaders: [[maskValue: true, name: 'Authorization', value: 'Bearer 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']], responseHandle: 'NONE', url: 'https://api.github.com/repos/mrunalgosar/gh-auto-labeler/pulls/2', wrapAsMultipart: false
    }
}