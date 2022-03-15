pipeline {
    agent any
    stages {
        stage('Clean & Install') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('Run!!!') {
            steps {
                sh "mvn spring-boot:run &"
            }
        }

        stage('Tutooooooo 20 segundos') {
            steps {
                sh 'sleep 20'
            }
        }

        stage('Test Newman') {
            steps {
                sh "newman run src/test/ejemplo-maven.postman_collection.json  -n 10 --delay-request 1000"
            }
        }

        stage('Jmeter') {
            steps {
                sh "mvn verify -Pperformance"
            }
        }
    }
}