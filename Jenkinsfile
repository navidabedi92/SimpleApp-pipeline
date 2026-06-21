stage('Test') {
    steps {
        dir('app') {
            script {
                def testExitCode = sh(
                    script: '''
                        dotnet test --configuration Release --no-build \
                            --logger "trx;LogFileName=test-results.trx" \
                            --results-directory ./TestResults
                    ''',
                    returnStatus: true
                )

                if (testExitCode == 0) {
                    echo 'All tests passed.'
                    // run your "if ok" command here
                    sh 'echo "Tests OK — proceeding with deployment step"'
                } else {
                    echo 'Tests failed.'
                    // run your "if not ok" command here
                    sh 'echo "Tests failed — sending notification"'
                    error("Failing pipeline due to test failures") // optional: marks build as FAILURE
                }
            }
        }
    }
}
