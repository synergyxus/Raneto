def git_commit = ''
def image = ''
pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr:'3'))
    }


    stages {
        stage("Docker Build") {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-creds') {
                        image = docker.build("synergyx/demo-nodejs-app")
                    }
                }
            }
        }

        stage("Publish to Docker Hub") {
            steps {
                script {
                    git_commit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    image.push(git_commit)
                    image.push("latest")
                }
            }
        }

    
    }
}
