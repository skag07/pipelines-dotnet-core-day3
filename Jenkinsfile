pipeline {
    agent any

     environment {

        AWS_ACCESS_KEY_ID     = credentials('Saeed-AccessKey')
        AWS_SECRET_ACCESS_KEY = credentials('Saeed-SecretKey')

        AWS_S3_BUCKET = "sda-learning-jenkins"
        ARTIFACT_NAME = "pipelines-dotnet-core-day3-A2.dll"
        AWS_EB_APP_NAME = "Saeed-Jenkins-assignment"
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        AWS_EB_ENVIRONMENT = "Saeedjenkinsassignment-env"

        SONAR_IP = "54.226.50.200"
        SONAR_TOKEN = "sqp_812ac7f458a01e0e751d4ab646a47cbfb23d21c4"

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

       
        stage('Package') {
            steps {
                
                sh "dotnet publish"

            }

            post {
                success {
                    archiveArtifacts artifacts: 'bin/Debug/net6.0/pipelines-dotnet-core-day3-A2.dll', followSymlinks: false
       
                }
            }
        }

        stage('Publish artefacts to S3 Bucket') {
            steps {

                sh "aws configure set region us-east-1"

                sh "aws s3 cp ./bin/Debug/net6.0/pipelines-dotnet-core-day3-A2.dll s3://$AWS_S3_BUCKET/$ARTIFACT_NAME"
                
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
