import groovy.json.JsonSlurperClassic
def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    stages {
        stage("Paso 1: Run SonarQube"){
            steps {
                script {
                sh "echo 'Run SonarQube!'"
                sh "mvn verify sonar:sonar \
                    -Dsonar.projectKey=ejemplo-maven \
                    -Dsonar.host.url=http://sonarqube:9000 \
                    -Dsonar.login=48b437692f02f70d38420ef42f7a99c203613c7f"
                }
            }
        }
        stage('SonarQube analysis') {
            withSonarQubeEnv(credentialsId: '48b437692f02f70d38420ef42f7a99c203613c7f', installationName: 'SonarQube') { // You can override the credential to be used
                sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
        }
        stage("Paso 2: Compilar"){
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean compile -e"
                }
            }
        }
        stage("Paso 3: Testear"){
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean test -e"
                }
            }
        }
        stage("Paso 4: Build .Jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "mvn clean package -e"
                }
            }
        }
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