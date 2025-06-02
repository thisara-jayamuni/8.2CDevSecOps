pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/thisara-jayamuni/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Allows pipeline to continue despite test failures
            }
              post {
                always {
                    emailext (
                        subject: "Jenkins: Run Tests stage completed - ${currentBuild.currentResult}",
                        body: "The 'Run Tests' stage finished with status: ${currentBuild.currentResult}. Please check Jenkins for full details.",
                        recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                        to: 'thisara.jayamuni95@gmail.com',
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext (
                        subject: "Jenkins: NPM Audit stage completed - ${currentBuild.currentResult}",
                        body: "The 'NPM Audit' stage finished with status: ${currentBuild.currentResult}. Please check Jenkins for full details.",
                        recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                        to: 'thisara.jayamuni95@gmail.com',
                        attachLog: true
                    )
                }
            }
        }

    }
}
