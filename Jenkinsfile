pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3001:3000 -p 5000:5000'
        }
    }
    environment {
        CI = 'true'
        HOME = '.'
    }

    tools { nodejs "node-v12" }
    stages {
        stage('Build') {
            environment {
               JENKINS_PATH = sh(script: 'pwd', , returnStdout: true).trim()
            }
            steps {
               echo "Hello world"
               echo "PATH=${JENKINS_PATH}"
               sh 'echo "JP=$JENKINS_PATH"'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'dev'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
