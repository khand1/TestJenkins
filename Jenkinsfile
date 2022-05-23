pipeline {
    agent none

    environment {
        DOCKER_IMAGE = "testjenkins"
    }

    stages {
        stage("build"){
            agent { node {label 'master'}}
            environment {
                DOCKER_TAG="${GIT_BRANCH.tokenize('/').pop()}-${GIT_COMMIT.substring(0,7)}"
            }
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} . "
                sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
                sh "docker image ls | grep ${DOCKER_IMAGE}"
                sh "docker container stop ${DOCKER_IMAGE}"
                sh "docker container rm ${DOCKER_IMAGE}"
                sh "docker run -p 7124:80 --name ${DOCKER_IMAGE} -d ${DOCKER_IMAGE}:latest"
                sh "docker rmi \$(docker images '${DOCKER_IMAGE}' -f 'before=${DOCKER_IMAGE}' -q)"
                sh "docker image prune -a"
                sh "y"
            }
        }
    }

    post {
        success {
            echo "SUCCESSFUL"
        }
        failure {
            echo "FAILED"
        }   
    }
}