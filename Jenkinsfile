import org.jenkinsci.plugins.github.pullrequest.GitHubPRCause

node( 'built-in' ) {
  println 'hello world'

  Boolean isPR = env?.GITHUB_PR_TARGET_BRANCH ?: false
  // com.cloudbees.jenkins.GitHubPushCause
  // org.jenkinsci.plugins.github.pullrequest.GitHubPRCause
  def triggerCause = currentBuild.rawBuild.getCause( GitHubPRCause.class )
  println """
    isPR: ${isPR}
    triggerCause : ${triggerCause}
  """


  try {
    cleanWs()
    if ( triggerCause ) {
        gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" )
        println ">> Build was started by ${triggerCause.userLogin}, who reviewed the PR: " +
                "\"${triggerCause.state}\", which matches one of " +
                "\"${triggerCause.reviewStates}\" trigger pattern."
    } else {
        println '>> Build was not started by github pull request.'
    }

    checkout scmGit(
      branches: [[name: '*/main']],
      browser: github('https://github.com/marslojiao-mvl/webhook.mvl'),
      extensions: [],
      userRemoteConfigs: [[credentialsId: 'GITHUB_SSH_CREDENTAIL', url: 'git@github.com:marslojiao-mvl/webhook.mvl.git']]
    )
    // error( '>> exit' )
  } catch ( Exception e ) {
    if ( triggerCause ) { githubPRAddLabels labelProperty: labels( 'ERROR' ) }
  } finally {
    if ( triggerCause ) {
      // println '>> 1:'
      // 'SUCCESS' == currentBuild.currentResult
      //   ? githubPRAddLabels ( labelProperty: labels( 'VERIFIED' ) )
      //   : githubPRAddLabels ( labelProperty: labels( 'FAILED' ) )

      println '>> 2:'
      githubPRComment comment: githubPRMessage( "${env.GITHUB_PR_HEAD_SHA} ${currentBuild.currentResult} in [${currentBuild.fullDisplayName}](${env.BUILD_URL})" )

      println '>> 3:'
      setGitHubPullRequestStatus context: "PR #${env.GITHUB_PR_NUMBER} ${currentBuild.currentResult} in ${currentBuild.fullDisplayName}",
                                 message: "CI build successfully in build ${currentBuild.fullDisplayName} ${BUILD_DISPLAY_NAME}",
                                 state: 'SUCCESS'
      println '>> 4:'
      publishChecks name: 'jenkins', summary: 'jenkins checks', text: 'jenkins', title: 'jenkins'

      println '>> 5:'
      githubPRStatusPublisher buildMessage: message(failureMsg: githubPRMessage('build failed.'), successMsg: githubPRMessage('build succeeded.')), errorHandler: statusOnPublisherError('FAILURE'), statusMsg: githubPRMessage("Build #${BUILD_NUMBER} ended"), unstableAs: 'FAILURE'

      println '>> 6:'
      step([ $class: 'GitHubCommitStatusSetter',
             statusResultSource: [
               $class: 'ConditionalStatusResultSource',
               results: [
                          [
                            $class  : 'BetterThanOrEqualBuildResult',
                            message : 'passed in ${currentBuild.fullDisplayName}',
                            result  : 'SUCCESS',
                            state   : 'SUCCESS'
                          ],
                          [
                            $class  : 'AnyBuildResult',
                            message : '${currentBuild.fullDisplayName} failed',
                            state   : 'FAILURE'
                          ]
               ]
             ]
      ])

    }
  } // try | finally
} // node

// gitHubPRStatus githubPRMessage( "${env.GITHUB_PR_COND_REF} run started" )
// githubPRStatusPublisher buildMessage: message( failureMsg: githubPRMessage('Build failed.  (Status set failed.)'),
                                               // successMsg: githubPRMessage('Build succeeded. (Status set Success.)')
                                      // ),
                        // statusMsg: githubPRMessage( "PR #${env.GITHUB_PR_NUMBER} ${currentBuild.currentResult} in #${env.BUILD_NUMBER}" )
// githubPRAddLabels labelProperty: labels( 'SUCCESS' ), statusVerifier: allowRunOnStatus( 'SUCCESS' )
// githubPRRemoveLabels labelProperty: labels('VERIFIED'), statusVerifier: allowRunOnStatus('FAILURE')
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
