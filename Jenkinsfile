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

        stage('SonarQube analysis'){
            environment {
                scannerHome = tool 'SonarScanner 7.0.2';
            }
            steps{
                withSonarQubeEnv('Sonar'){
                    sh "${scannerHome}/bin/sonar-scanner -D sonar.projectName=wonderlust -D sonar.projectKey=squ_6e2c2f732aa7d0936bbf473ec983626aeace08e5 -D sonar.sources=."
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