println 'hello world'

node( 'built-in' ) {
  cleanWs()
  checkout scmGit(
    branches: [[name: '*/main']],
    browser: github('https://github.com/marslojiao-mvl/webhook.mvl'),
    extensions: [],
    userRemoteConfigs: [[credentialsId: 'GITHUB_SSH_CREDENTAIL', url: 'git@github.com:marslojiao-mvl/webhook.mvl.git']]
  )

  if ( env?.GITHUB_PR_TARGET_BRANCH ?: false ) {
    gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" )
    githubPRComment comment: githubPRMessage( "#${env.BUILD_NUMBER} ${currentBuild.currentResult}" )
    githubPRAddLabels labelProperty: labels( 'SUCCESS' ), statusVerifier: allowRunOnStatus( 'SUCCESS' )
    githubPRClosePublisher statusVerifier: allowRunOnStatus('SUCCESS')
  }
}

