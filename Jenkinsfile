pipeline {
    agent {
        label 'nodejs-slave'
    }
    // environment {
    //     DOCKER_IMAGE = 'react-app'
    //     DOCKER_TAG = 'latest'
    // }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        // stage('Install Dependencies') {
        //     steps {
        //         // dir('react-app') {
        //         // }
        //             sh 'npm install'
        //     }
        // }
        // stage('Build') {
        //     steps {
        //         // dir('react-app') {
        //         // }
        //             sh 'npm run build'
        //     }
        // }
        stage('Docker Build & Push') {
            steps {
                script {
                    def commitHash = sh(script: 'git rev-parse --short=6 HEAD', returnStdout: true).trim()
                    def dockerImage = "phuongtn20/frontend-app:${commitHash}"

                    withCredentials([usernamePassword(credentialsId: 'phuongtn20-docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                        sh "docker build -t ${dockerImage} ."
                        sh "docker push ${dockerImage}"
                    }
                }
            }
        }
        // stage('Test') {
        //     steps {
        //         // dir('react-app') {
        //         // }
        //             sh 'npm test -- --watchAll=false'
        //     }
        // }

        // stage('Deploy') {
        //     steps {
        //         dir('react-app') {
        //             sh '''
        //                 npm install -g pm2
        //                 npm install -g http-server

        //                 pm2 delete react-app || true
        //                 pm2 start $(which http-server) --name "react-app" -- ./build -p 3000 --proxy http://localhost:3000
        //                 pm2 status
        //             '''
        //         }
        //     }
        // }
        // stage('Build Docker Image') {
        //     steps {
        //         // dir('react-app') {
        //         // }
        //         sh 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
        //     }
        // }
        // stage('Deploy') {
        //     steps {
        //         sh '''
        //             docker stop react-app || true
        //             docker rm react-app || true
        //             docker run -d --name react-app -p 3000:80 ${DOCKER_IMAGE}:${DOCKER_TAG}
        //         '''
        //     }
        // }
        // stage('Archive') {
        //     steps {
        //         // dir('react-app') {
        //         // }
        //             archiveArtifacts artifacts: 'build/**/*', fingerprint: true
        //     }
        // }
    }
        post {
            success {
                echo 'Frontend Docker image pushed successfully.'
            }
        }
}
