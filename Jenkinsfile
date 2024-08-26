
pipeline {
    // agent {
    //     docker {
    //         image 'node:14'
    //     }
    // }
    agent any
    environment {
        REGISTRY_CREDENTIALS = credentials('dockerhub-credentials') // Jenkins credentials ID for Docker Hub
        DOCKER_IMAGE = 'praveen431ece/samplenodejs'
        NODE_DOCKER_IMAGE = 'node:14' // Specify the Node.js version you want to use
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'demo', url: 'https://github.com/praveenece431/sample-node-project.git'
            }
        }
        
        stage('Build') {
            // agent {
            // docker { image 'node:14' }
            // }
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        
        stage('Unit Tests') {
            steps {
                script {
                    // sh 'npm test'
                    '''
                    echo 'Running Unit Tests'
                    npm test
                    sleep 5
                    '''
                }
            }
        }
        
        stage('SonarQube Analysis') {

            steps {
                script {
                    '''
                    echo 'Running SonarQube Analysis'
                    echo 'This is a dummy stage'
                    sleep 5
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        sh 'docker push $DOCKER_IMAGE:${BUILD_NUMBER}'
                    }
                }
            }
        }

        stage('Docker Remove Running Containers') {
            steps {
                script {
                    sh '''
                    docker stop $(docker ps -aq)
                    docker rm $(docker ps -aq)
                    '''
                }
            }
        }

        stage('Docker Run New Container') {
            steps {
                script {
                    sh 'docker run -d -p 3000:3005 $DOCKER_IMAGE:${BUILD_NUMBER}'
                }
            }
        }
    }
    post {
        always {
            cleanWs() // Cleanup the workspace
        }
    }
}
