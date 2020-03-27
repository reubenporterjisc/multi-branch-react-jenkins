pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000' 
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Tests') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Lighthouse report') {
            steps {
                sh 'npm install -g @lhci/cli@0.3.x'
                sh './jenkins/scripts/deliver-for-development.sh'
                sh 'lhci autorun'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deliver to development server') {
            when {
                branch 'master'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                sh 'lhci autorun'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy to staging server') {
            when {
                branch 'production'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}