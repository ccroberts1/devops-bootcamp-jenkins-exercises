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
                        echo "Building the app..."
                    }
        }
        stage("build image") {
                    steps {
                        echo "Building the image..."
                    }
        }
        stage("deploy") {
                    steps {
                        echo "Deploying the app..."
                    }
        }
    }
}