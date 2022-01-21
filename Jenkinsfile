pipeline {
    agent any
    stages {
        stage("1: Compile"){
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                //sh "mvn clean compile -e"
                }
            }
        }
        stage("2: Unit Test"){
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                //sh "mvn clean test -e"
                }
            }
        }
        stage("3: Build jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                //sh "mvn clean package -e"
                }
            }
        }
        stage("4: SonarQube"){
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh "echo 'SonarQube'"
                    // Run Maven on a Unix agent to execute Sonar.
                    //sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=githubfull'
                }
            }
        }
        stage("5: Nexus Upload"){
            steps {
                script {
                sh "echo 'Nexus Upload'"
                // Run Maven on a Unix agent.
                //sh "mvn clean test -e"
                }
            }
        }
        stage("6: gitCreateRelease"){
            steps {
                script {
                sh "echo 'gitCreateRelease'"
                // Run Maven on a Unix agent.
                //sh "mvn clean test -e"
                }
            }
        }
        // stage("Paso 5: Levantar Springboot APP"){
        //     steps {
        //         sh 'mvn spring-boot:run &'
        //     }
        // }
        // stage("Paso 6: Dormir(Esperar 10sg) "){
        //     steps {
        //         sh 'sleep 60'
        //     }
        // }
        // stage("Paso 7: Test Alive Service - Testing Application!"){
        //     steps {
        //         sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'
        //     }
        // }

    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
        success {
            sh "echo 'fase success'"
        }
        failure {
            sh "echo 'fase failure'"
        }
    }
}