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
    githubPRComment comment: githubPRMessage( "${env.GITHUB_PR_HEAD_SHA} in build #${env.BUILD_NUMBER} marked as ${currentBuild.currentResult}" +
                                              "<details><summary>Details</summary><p>[Jenkins Build](${env.BUILD_URL})</p></details>"
                                            ),
                    errorHandler: statusOnPublisherError( 'UNSTABLE' )

    githubPRAddLabels errorHandler: statusOnPublisherError( 'UNSTABLE' ), labelProperty: labels( 'verified' )

    githubPRStatusPublisher buildMessage: message( failureMsg: githubPRMessage('Build failed.  (Status set failed.)'),
                                                   successMsg: githubPRMessage('Build succeeded. (Status set Success.)')
                                          ),
                            statusMsg: githubPRMessage( "PR #${env.GITHUB_PR_NUMBER} ${currentBuild.currentResult} in #${env.BUILD_NUMBER}" )
    gitHubPRStatus githubPRMessage( "PR #${env.GITHUB_PR_NUMBER} ${currentBuild.currentResult} in #${env.BUILD_NUMBER}" )


    // gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" )
    // githubPRComment comment: githubPRMessage( "#${env.BUILD_NUMBER} ${currentBuild.currentResult}" )
    // githubPRAddLabels labelProperty: labels( 'SUCCESS' ), statusVerifier: allowRunOnStatus( 'SUCCESS' )
    // githubPRClosePublisher statusVerifier: allowRunOnStatus('SUCCESS')
  }
}

// void setBuildStatus( String message, String state ) {
//   step([
//       $class: "GitHubCommitStatusSetter",
//       reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/marslojiao-mvl/webhook.mvl"],
//       contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
//       errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
//       statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
//   ]);
// }
