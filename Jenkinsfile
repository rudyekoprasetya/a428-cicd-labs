pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input(
                        message: 'Lanjutkan ke tahap Deploy?',
                        parameters: [choice(choices: ['Yes'], description: 'Pilih Salah satu', name: 'CONTINUE')]
                    )
                    if(userInput == 'Yes') {
                        echo 'Melanjutkan deployment..'
                    } else {
                        echo 'Deployment dibatalkan..'
                        sh './jenkins/scripts/kill.sh'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deployment sedang berlangsung...'
                sh './jenkins/scripts/deliver.sh'
                sh 'sleep 60'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
