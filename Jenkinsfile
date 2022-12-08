pipeline{
    agent any
    tools{
        maven  'M3'
    }
    parameters{
        string( defaultValue: 'Test', name: 'environment')
    }
    stages {

        stage ('GIT Checkout') {
            steps{
                echo 'Checking out source Code from Repo...'
                echo "The Environment for Job Execution is ${params.environment}"
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '6fcf4fdc-2307-4ed0-aece-f277e0c30202', url: 'https://github.com/GreenStar01/JAVA-DEMO.git']]])
            }

        }
        stage ('Pre-build') {
            steps{
                bat 'mvn clean compile'
            }
        }

        stage ('UnitTest'){
            steps{
                bat 'mvn test'
            }
        }

        stage ('Test-Report'){
            steps{
                junit '**/target/surefire-reports/TEST-*.xml'
                jacoco()
            }
        }

        stage ('Build') {
            steps {
                bat 'mvn package '
            }
        }
        stage ('Integration-Test') {
            steps{
                bat ('mvn integration-test')
            }
        }
        stage ('Archive Output'){
            steps{
                archiveArtifacts artifacts: '**/*.jar', onlyIfSuccessful: true
            }
        }



    }
}

