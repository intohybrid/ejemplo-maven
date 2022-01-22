pipeline {
    agent any
    environment{
        NEXUS_USER = credentials('usernexusadmin')
        NEXUS_PASSWORD = credentials('passnexusadmin')
        VERSION = '0.0.14'
        FINAL_VERSION = '1.0.0'
    }
    stages {
        // stage("Paso 1: Compliar"){
        //     steps {
        //         script {
        //         sh "echo 'Compile Code!'"
        //         // Run Maven on a Unix agent.
        //         sh "mvn clean compile -e"
        //         }
        //     }
        // }
        // stage("Paso 2: Testear"){
        //     steps {
        //         script {
        //         sh "echo 'Test Code!'"
        //         // Run Maven on a Unix agent.
        //         sh "mvn clean test -e"
        //         }
        //     }
        // }
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "mvn clean package -e"
                }
            }
        }
        stage("Paso 4: Análisis SonarQube"){
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh "echo 'Calling sonar Service in another docker container!'"
                    // Run Maven on a Unix agent to execute Sonar.
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=githubfull'
                }
            }
            post {
                //record the test results and archive the jar file.
                success {
                    nexusPublisher nexusInstanceId: 'nexus', 
                    nexusRepositoryId: 'devops-usach-nexus', 
                    packages: [[$class: 'MavenPackage', 
                        mavenAssetList: [[classifier: '', 
                                        extension: '',
                                        filePath: 'build/DevOpsUsach2020-0.0.1.jar']],
                        mavenCoordinate: [artifactId: 'DevOpsUsach2020', 
                                        groupId: 'com.devopsusach2020', 
                                        packaging: 'jar', 
                                        version: VERSION]]]
                }
            }
        }
        stage("Download: Nexus"){                                   
            steps {                   
                sh 'sleep 5 '
                sh 'curl -X GET -u $NEXUS_USER:$NEXUS_PASSWORD http://nexus:8081/repository/devops-usach-nexus/com/devopsusach2020/DevOpsUsach2020/$VERSION/DevOpsUsach2020-$VERSION.jar -O'
            }
        }
        stage("Run: Levantar Springboot APP"){
            steps {
                sh 'nohup java -jar DevOpsUsach2020-$VERSION.jar & >/dev/null'
            }
        }
        stage("Curl: Dormir(Esperar 30sg) "){
            steps {
               sh "sleep 30 && curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'"
            }
        }
        stage("Subir nueva Version"){
            steps {
                nexusPublisher nexusInstanceId: 'nexus',
                    nexusRepositoryId: 'devops-usach-nexus',
                    packages: [
                        [$class: 'MavenPackage',
                            mavenAssetList: [
                                [classifier: '',
                                extension: 'jar',
                                filePath: 'DevOpsUsach2020-${VERSION}.jar']
                            ],
                    mavenCoordinate: [
                        artifactId: 'DevOpsUsach2020',
                        groupId: 'com.devopsusach2020',
                        packaging: 'jar',
                        version: FINAL_VERSION]
                    ]
                ]
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