println 'hello world'

node( 'built-in' ) {
  cleanWs()
  checkout scmGit(
    branches: [[name: '*/main']],
    browser: github('https://github.com/marslojiao-mvl/webhook.mvl'),
    extensions: [],
    userRemoteConfigs: [[credentialsId: 'GITHUB_SSH_CREDENTAIL', url: 'git@github.com:marslojiao-mvl/webhook.mvl.git']]
  )
}
