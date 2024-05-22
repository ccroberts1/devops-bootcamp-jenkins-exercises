#!/user/bin/env groovy
@Library('jenkins-shared@main')_

pipeline {
    agent any
    tools {
        nodejs "node"
    }
    stages {
        stage("test") {
                steps {
                    dir("app"){
                        script {
                            test()
                        }
                    }
                }
        }
        stage("increment version") {
            steps {
                dir("app") {
                    script {
                        incrementVersion()
                    }
                }
            }
        }
        stage("build image") {
            steps {
                script {
                   buildImage()
                }
            }
        }
        stage("commit version update") {
            steps {
                script {
                    commitVersionUpdate()
                }
            }
        }
    }
}