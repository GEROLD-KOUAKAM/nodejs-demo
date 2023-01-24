    pipeline {
        agent any
        environment {
            DOCKERHUB_CREDENTIALS = credentials('dockerhub')
             AWS_ACCOUNT_ID="117166837117"
             AWS_DEFAULT_REGION="us-east-1"
             IMAGE_REPO_NAME="app-jenkins"
             IMAGE_TAG="v1"
             REPOSITORY_URI = "117166837117.dkr.ecr.us-east-1.amazonaws.com/app-jenkins"
        }
        stages {
            stage('Build docker images') {
                steps {
                    sh 'docker build -t $IMAGE_REPO_NAME/$IMAGE_TAG:$BUILD_NUMBER .'
                }
            }

              stage('Logging into AWS ECR') {
            steps {
                script {
                sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
                 
            }
        }

            /*
            stage('login to dockerhub') {
                steps{
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
            */

             stage('push image to ecr') {
                steps{
                     sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                     sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
                     
                     //sh 'docker push $IMAGE_REPO_NAME/$IMAGE_TAG:$BUILD_NUMBER'
                }
            }
            /*
            stage('push image to dockerhub') {
                steps{
                    sh 'docker push geroldsiewe/nodeapp:$BUILD_NUMBER'
                }
            }*/


        }
        post {
            always {
                sh 'docker logout'
            }
        }
    }


