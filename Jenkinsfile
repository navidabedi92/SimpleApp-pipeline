pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:10.0'
            args '-u root' // avoids permission issues with NuGet cache/tmp dirs
        }
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = '1'
        NUGET_PACKAGES = "${WORKSPACE}/.nuget/packages" // keep NuGet cache inside workspace
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    dotnet test --configuration Release --no-build \
                        --logger "trx;LogFileName=test-results.trx" \
                        --results-directory ./TestResults
                '''
            }
        }
    }

    post {
        always {
            junit testResults: 'TestResults/*.trx', allowEmptyResults: true
            cleanWs()
        }
    }
}
