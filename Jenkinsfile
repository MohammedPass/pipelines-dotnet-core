pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
        AWS_S3_BUCKET = "repo-backet"
        ARTIFACT_NAME = "hello-world.dll"
        AWS_EB_APP_NAME = "mohammedeid-.NET-webapp"
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        AWS_EB_ENVIRONMENT = "Mohammedeidnetwebapp-env"

    

    }

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
        }
        stage('Quality Scan'){
            steps {
                sh '''
                                dotnet tool restore
                dotnet sonarscanner begin /k:"Mohammed_Eid-dotnet" /d:sonar.host.url="http://54.226.50.200"  /d:sonar.login="sqp_f9427808e579878aec930fe023e2d4c868afdcfa"
                dotnet build
                dotnet sonarscanner end /d:sonar.login="sqp_f9427808e579878aec930fe023e2d4c868afdcfa"
                '''
            }
        }

        stage('Publish') {
            steps {
                sh "dotnet publish"
            }
            post{
                success{
                    archiveArtifacts artifacts: '**/bin/Debug/net6.0/**.dll', followSymlinks: false

                    
                }
            }
        }
        stage('Publish artifacts to S3 Bucket') {
            steps {
                sh "aws configure set region us-east-1"
                sh "aws s3 cp ./bin/Debug/net6.0/pipelines-dotnet-core.dll s3://$AWS_S3_BUCKET/$ARTIFACT_NAME"
            }
        }
        stage('Deploy') {
            steps {
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
            }
        }
        

    }
}