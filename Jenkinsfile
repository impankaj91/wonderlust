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

        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }

        stage('checkout code'){
            steps{
                script {
                    checkout_code('https://github.com/impankaj91/wonderlust.git','main')
                }
            }
        }

        stage('File Scan') {
            steps {
                script {
                    trivy()
                }
            }
        }

        //stage('OWASP Dependency Checks') {
        //    steps {
        //        script {
        //            owasp_dependency_check()
        //        }
        //    }
        //}

        stage('SonarQube analysis'){
            environment {
                scannerHome = tool 'SonarScanner 7.0.2';
            }

            steps {
                script {
                    sonar_analysis('Sonar','wonderlust','squ_6e2c2f732aa7d0936bbf473ec983626aeace08e5')
                }
            }
        }

        stage('Build(Docker)') {
            steps{
                dir('frontend') {
                script{
                    docker_build('wonderlust-frontend',"${BUILD_NUMBER}")
                }
                }
                dir('backend') {
                script{
                    docker_build('wonderlust-backend',"${BUILD_NUMBER}")
                }
                }
            }
        }

        stage('Docker Tag Latest') {
            steps {
                sh """

                docker tag wonderlust-frontend:${BUILD_NUMBER} wonderlust-frontend:latest
                docker tag wonderlust-backend:${BUILD_NUMBER} wonderlust-backend:latest 

                """
            }

        stage('Docker Push') {
            steps {
                script {
                    docker_push('DockerHub','wonderlust-frontend',"${BUILD_NUMBER}")
                    docker_push('DockerHub','wonderlust-backend',"${BUILD_NUMBER}")
                    docker_push('DockerHub','wonderlust-frontend',"latest")
                    docker_push('DockerHub','wonderlust-backend',"latest")
                }
            }
        }
    }
}