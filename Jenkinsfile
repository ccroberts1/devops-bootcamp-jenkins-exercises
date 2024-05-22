pipeline {
    agent any
    stages {
        stage("test") {
                steps {
                    echo "Testing the app..."
                }
        }
        stage("increment version") {
            steps {
                echo "Incrementing app version..."
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