println 'hello world'

node( 'built-in' ) {
  Boolean isPR = env?.GITHUB_PR_TARGET_BRANCH ?: false
  try {
    cleanWs()
    if ( isPR ) { gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" ) }
    checkout scmGit(
      branches: [[name: '*/main']],
      browser: github('https://github.com/marslojiao-mvl/webhook.mvl'),
      extensions: [],
      userRemoteConfigs: [[credentialsId: 'GITHUB_SSH_CREDENTAIL', url: 'git@github.com:marslojiao-mvl/webhook.mvl.git']]
    )
  } finally {
    if ( isPR ) {
      githubPRComment comment: githubPRMessage( "${env.GITHUB_PR_HEAD_SHA} ${currentBuild.currentResult} in [${currentBuild.fullDisplayName}](${env.BUILD_URL})" )
      setGitHubPullRequestStatus context: "PR #${env.GITHUB_PR_NUMBER} ${currentBuild.currentResult} in ${currentBuild.fullDisplayName}",
                                 message: "CI build successfully in build ${currentBuild.fullDisplayName} ${BUILD_DISPLAY_NAME}",
                                 state: 'SUCCESS'
      'SUCCESS' == currentBuild.currentResult ? githubPRAddLabels ( labelProperty: labels( 'VERIFIED' ) ) : githubPRAddLabels ( labelProperty: labels( 'FAILED' ) )
    }
  } // try | finally
} // node

// gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" )
// githubPRStatusPublisher buildMessage: message( failureMsg: githubPRMessage('Build failed.  (Status set failed.)'),
                                               // successMsg: githubPRMessage('Build succeeded. (Status set Success.)')
                                      // ),
                        // statusMsg: githubPRMessage( "PR #${env.GITHUB_PR_NUMBER} ${currentBuild.currentResult} in #${env.BUILD_NUMBER}" )
// githubPRAddLabels labelProperty: labels( 'SUCCESS' ), statusVerifier: allowRunOnStatus( 'SUCCESS' )
// githubPRClosePublisher statusVerifier: allowRunOnStatus('SUCCESS')

// void setBuildStatus( String message, String state ) {
//   step([
//       $class: "GitHubCommitStatusSetter",
//       reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/marslojiao-mvl/webhook.mvl"],
//       contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
//       errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
//       statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
//   ]);
// }
