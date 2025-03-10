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
    }
}