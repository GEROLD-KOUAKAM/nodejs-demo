    pipeline {
        agent any
        environment {
            DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        }
        stages {
            stage('Build docker image') {
                steps {
                    sh 'docker build -t geroldsiewe/nodeapp:$BUILD_NUMBER .'
                }
            }
            stage('login to dockerhub') {
                steps{
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
            stage('push image to dockerhub') {
                steps{
                    sh 'docker push geroldsiewe/nodeapp:$BUILD_NUMBER'
                }
            }
        }
        post {
            always {
                sh 'docker logout'
            }
        }
    }


