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
                 sh 'echo .env.BRANCH_NAME'
            }
        }
        stage('Test') {
            steps {
                 sh 'echo .env.BRANCH_NAME'
                sh './jenkins/scripts/test.sh'
            
            }
        }
        stage('Lighthouse report') {
            steps {
                sh 'npm install -g @lhci/cli@0.3.x'
                sh 'lhci autorun'
            }
        }
        stage('Deliver to development server') {
            when {
                branch 'development'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
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