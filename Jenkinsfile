pipeline {
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timestamps()
    }
    agent {
        label 'docker-v18.03'
    }
    stages {
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
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        success {
            logstashPush("SUCCESS")
        }
        failure {
            logstashPush("FAILURE")
        }
        unstable {
            logstashPush("UNSTABLE")
        }
        always {
            deleteDir()
        }
    }
}