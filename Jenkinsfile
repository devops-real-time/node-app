pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
        NEXUS_URL  = "http://18.141.202.202:9090"
        IMAGE_URL_WITH_TAG = "${NEXUS_URL}/node-app:${DOCKER_TAG}"
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t ${IMAGE_URL_WITH_TAG}"
            }
        }
        stage('Nexus Push'){
            steps{
                withCredentials([string(credentialsId: 'nexus-pwd', variable: 'nexusPwd')]) {
                    sh "docker login -u admin -p ${nexusPwd} ${NEXUS_URL}"
                    sh "docker push ${IMAGE_URL_WITH_TAG}"
                }
            }
	}
    }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
