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
                            echo "Testing the app..."
                            sh "npm install"
                            sh "npm run test"
                        }
                    }
                }
        }
        stage("increment version") {
            steps {
                dir("app") {
                    script {
                        echo "Incrementing app version..."
                        sh "npm version minor"
                        def packageJson = readJSON file: 'package.json'
                        def version = packageJson.version

                        env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                    }
                }
            }
        }
//         stage("build app") {
//             steps {
//                 dir("app") {
//                     script {
//                         echo "Building the app..."
//                         sh "npm build"
//                     }
//                 }
//             }
//         }
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
                dir("app") {
                    script {
                        echo "Deploying the app..."
                        sh "node server.js"
                    }
                }
            }
        }
        stage("commit version update") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'

                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/ccroberts1/devops-bootcamp-jenkins-exercises.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                    }
                }
            }
        }
    }
}