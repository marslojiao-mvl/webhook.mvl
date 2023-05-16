println 'hello world'

node( 'built-in' ) {
  cleanWs()
  checkout scmGit(
    branches: [[name: '*/main']],
    browser: github('https://github.com/marslojiao-mvl/webhook.mvl'),
    extensions: [],
    userRemoteConfigs: [[credentialsId: 'GITHUB_SSH_CREDENTAIL', url: 'git@github.com:marslojiao-mvl/webhook.mvl.git']]
  )

  gitHubPRStatus githubPRMessage( '${GITHUB_PR_COND_REF} run started' )
  githubPRComment comment: githubPRMessage( '''Build ${BUILD_NUMBER} ${BUILD_STATUS}''' ),
                  errorHandler: statusOnPublisherError( 'FAILURE' )

  githubPRStatusPublisher buildMessage: message( failureMsg: githubPRMessage('Can\'t set status; build failed.'), successMsg: githubPRMessage('Can\'t set status; build succeeded.') ),
                          errorHandler: statusOnPublisherError( 'FAILURE' ),
                          statusMsg: githubPRMessage( '${GITHUB_PR_COND_REF} run ended' ),
                          unstableAs: 'FAILURE'
}
