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

    githubPRComment comment: githubPRMessage( "Build #${env.BUILD_NUMBER} marked as ${currentBuild.currentResult}" +
                                              "<details><summary>Details</summary><p>[Jenkins Build](${env.BUILD_URL})</p></details>"
                                            ),
                    errorHandler: statusOnPublisherError( 'UNSTABLE' )

    githubPRAddLabels errorHandler: statusOnPublisherError( 'UNSTABLE' ), labelProperty: labels( 'approved' )

    githubPRStatusPublisher buildMessage: message( failureMsg: githubPRMessage('Build failed.  (Status set failed.)'),
                                                   successMsg: githubPRMessage('Build succeeded. (Status set Success.)')
                                          ),
                            errorHandler: statusOnPublisherError( 'UNSTABLE' ),
                            statusMsg: githubPRMessage( ('SUCCESS' == currentBuild.currentResult) ? "Successful in ${currentBuild.durationString}" : "Build failed in #${env.BUILD_NUMBER}" ),
                            unstableAs: 'FAILURE'

    // gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" )
    // githubPRComment comment: githubPRMessage( "#${env.BUILD_NUMBER} ${currentBuild.currentResult}" )
    // githubPRAddLabels labelProperty: labels( 'SUCCESS' ), statusVerifier: allowRunOnStatus( 'SUCCESS' )
    // githubPRClosePublisher statusVerifier: allowRunOnStatus('SUCCESS')
  }
}

