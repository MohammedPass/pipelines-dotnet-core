pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {

                sh "dotnet restore"

            }
        }

                stage('Test') {
            steps {

                sh "dotnet test"

            }

            
        }

        stage('Build') {
            steps {

                sh "dotnet build"

            }
        }



        stage('Package') {
            steps {

                sh "dotnet publish"

            }
                post{
                    success {
                        archiveArtifacts artifacts: '**/*.dll', followSymlinks: false
                    }
                }
        }
    }
} 