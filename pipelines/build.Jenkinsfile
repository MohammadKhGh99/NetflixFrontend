// pipelines/build.Jenkinsfile

pipeline {
    agent any

    triggers {
        githubPush()
    }

    options {
        timeout(time: 10, unit: 'MINUTES')  // discard the build after 10 minutes of running
        timestamps()  // display timestamp in console output
    }

    environment {
        // GIT_COMMIT = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
        // TIMESTAMP = new Date().format("yyyyMMdd-HHmmss")

        IMAGE_TAG = 0.0.${BUILD_NUMBER}
        IMAGE_BASE_NAME = 'netflix-app'

//         DOCKER_USERNAME = credentials('dockerhub').username
//         DOCKER_PASS = credentials('dockerhub').password
    }

    stages {
        stage('Docker setup') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      docker login -u $DOCKER_USERNAME -p $DOCKER_PASS
                    '''
                }
            }
        }

            stage('Build & Push') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                          IMAGE_FULL_NAME=$DOCKER_USERNAME/$IMAGE_BASE_NAME:$IMAGE_TAG

                          docker build -t $IMAGE_FULL_NAME .
                          docker push $IMAGE_FULL_NAME
                        '''
                    }
                }
            }
//         stage('Build app container') {
//             steps {
//                 sh '''
//                     # your pipeline commands here....
//
//                     # for example list the files in the pipeline workdir
//                     ls
//
//                     # build an image
//                     docker build -t netflix-front .
//                 '''
//             }
//         }
    }
}