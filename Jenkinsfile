pipeline {
    agent any
	stages{
		stage('Build') {
            steps {
                script {
                    sh "docker build -t hub.docker.com/palak06/vulnerability:v1 ."
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    sh "docker push hub.docker.com/palak06/vulnerability:v1"
                    sh "sleep 30"
                    sh "docker rmi \$(docker images -q)"
                    sh "docker images -a"
                    sh "sleep 30"
                    sh "docker pull hub.docker.com/palak06/vulnerability:v1"
                }
            }
        }
        stage('Analyze'){
            steps {
                script {
                    def imageLine = "hub.docker.com/palak06/vulnerability:v1"
                    writeFile file: 'anchore_images', text: imageLine
                    anchore name: 'anchore_images'
                }
            }
        }
    }
    post {
        success {
            echo 'SUCCESS'
        }
        failure {
            echo 'FAILURE'
        }
        unstable {
            echo 'UNSTABLE'
        }
        always {
            deleteDir()
        }
    }
}
