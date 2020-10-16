pipeline {
    agent any
	stages{
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
		stage('Build') {
            steps {
                script {
                    sh "docker build -t palak06/vulnerability:v1 ."
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    sh "docker push palak06/vulnerability:v2"
                    sh "sleep 30"
                    sh "docker rmi \$(docker images -q)"
                    sh "docker images -a"
                    sh "sleep 30"
                    sh "docker pull palak06/vulnerability:v2"
                }
            }
        }
        stage('Analyze'){
            steps {
                script {
                    def imageLine = "palak06/vulnerability:v2"
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
