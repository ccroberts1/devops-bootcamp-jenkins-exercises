pipeline {
    agent any
    stages {
        stage("test") {
                steps {
                    dir("app"){
                        script {
                            echo "Testing the app..."
                            sh "npm install"
                            sh "npm run test"
                        }
                    }
                }
        }
        stage("increment version") {
            steps {
                script {
                    echo "Incrementing app version..."
                    sh "npm version minor"
                    def packageJson = readJSON file: 'package.json'
                    def version = packageJson.version

                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage("build app") {
                    steps {
                        script {
                            echo "Building the app..."
                            sh "npm run build"
                        }
                    }
        }
        stage("build image") {
                    steps {
                        script {
                            echo "Building the Docker image..."
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                sh "docker build -t ccroberts1/demo-app:${IMAGE_NAME} ."
                                sh "echo $PASS | docker login -u ${USER} --password-stdin"
                                sh "docker push ccroberts1/demo-app:${IMAGE_NAME}"
                            }
                        }
                    }
        }
        stage("deploy") {
                    steps {
                        script {
                            echo "Deploying the app..."
                            sh "node server.js"
                        }
                    }
        }
    }
}