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
                    tag = "${branch_name}-${git_commit}"
                    image.push(tag)
                }
            }
        }
        stage("Run newly created Docker image in Kubernetes Cluster") {
            steps {
                script {
                    image_name = "synergyx/demo-nodejs-app:${tag}"
                    sh "kubectl set image deployment/demo-nodejs-app demo-nodejs-app=${image_name}"
                    sh "kubectl rollout status deployment/demo-nodejs-app"
                }
            }
        }
    
    }
}
