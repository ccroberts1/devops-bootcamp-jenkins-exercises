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
        stage("build image") {
            steps {
                script {
                    echo "Building the Docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t ccroberts1/demo-app:${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u ${USER} --password-stdin'
                        sh "docker push ccroberts1/demo-app:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage("commit version update") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        def encodedPassword = URLEncoder.encode("${PASS}", 'UTF-8')
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'

                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh "git push https://${USER}:${PASS}@github.com/${USER}/devops-bootcamp-jenkins-exercises.git HEAD:jenkins-jobs"
                    }
                }
            }
        }
    }
}