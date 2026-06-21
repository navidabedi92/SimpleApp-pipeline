pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:10.0'
            args '-u root'
        }
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = '1'
        NUGET_PACKAGES = "${WORKSPACE}/.nuget/packages"
    }

    stages {
        stage('Checkout App Code') {
            steps {
                dir('app') {
                    git url: 'https://github.com/navidabedi92/SimpleApp.git', branch: 'main'
                }
            }
        }

        stage('Restore') {
            steps {
                dir('app') {
                    sh 'dotnet restore'
                }
            }
        }

        stage('Build') {
            steps {
                dir('app') {
                    sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }

        stage('Test') {
            steps {
                dir('app') {
                    sh '''
                        dotnet test --configuration Release --no-build \
                            --logger "trx;LogFileName=test-results.trx" \
                            --results-directory ./TestResults
                    '''
                }
            }
        }
    }

    post {
        always {
            junit testResults: 'app/TestResults/*.trx', allowEmptyResults: true
            cleanWs()
        }
    }
}
