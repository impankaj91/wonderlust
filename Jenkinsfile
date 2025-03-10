@Library('Shared') _

pipeline {
    agent{ label 'linux'}
    stages{
        stage('Welcome Message'){
         steps{
            script {
                welcome()
            }
        }
        }
        stage('checkout code'){
            steps{
                script {
                    checkout_code('https://github.com/impankaj91/wonderlust.git','main')
                }
            }
        }

        stage('Build(Docker)') {
            steps{
                dir('frontend') {
                script{
                    docker_build('wonderlust-frontend','latest')
                }
                }
                dir('backend') {
                script{
                    docker_build('wonderlust-backend','latest')
                }
                }
            }
        }
    }
}