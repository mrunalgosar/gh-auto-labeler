node {
    stage('Checkout') {
        checkout scmGit(branches: [[name: '*/master']], browser: github('https://github.com/mrunalgosar/gh-auto-labeler.git'), extensions: [], userRemoteConfigs: [[credentialsId: '2bd6bfe8-2f4c-4b8f-9549-5f3d4e2496f1', url: 'https://github.com/mrunalgosar/gh-auto-labeler.git']])
    }
}