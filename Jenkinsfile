pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {

                sh "dotnet restore"

                

            }
        }

        stage('Build') {
            steps {

                sh "dotnet build"

            }
        }

        stage('Test') {
            steps {

                sh "dotnet test"

            }

            post {
                always {
                    junit '*/bin/Debug/net6.0/.dll'
                }
            }
        }
    }
} 