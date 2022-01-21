pipeline {
    agent any
    stages {
        stage("-1: logs"){
            steps {
                sh "echo 'git branch: '" + GIT_BRANCH
                sh "echo 'branchname: '" + BRANCH_NAME
                sh "echo 'targetEnv: '" + params.targetEnv
                sh 'printenv'
            }
        }
        stage("0: validate"){
            when { 
                expression { 
                    return !BRANCH_NAME ==~ /featuasdre\/[0-9]+\.[0-9]+\.[0-9]+/ 
                    }
                }
            steps {
                // Abort the build, skipping subsequent stages
                sh "echo  'nombre invalido'"
                 error("Invalid Name")
            } 
        }
        stage("1: Compile"){
            when {
                branch 'development' 
            }
        //- Compilar el código con comando maven
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                //sh "mvn clean compile -e"
                }
            }
        }
        stage("2: Unit Test"){
            when {
                branch 'development' 
            }
        //- Testear el código con comando maven
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                //sh "mvn clean test -e"
                }
            }
        }
        stage("3: Build jar"){
        //- Generar artefacto del código compilado.
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                //sh "mvn clean package -e"
                }
            }
        }
        stage("4: SonarQube"){
        //- Generar análisis con sonar para cada ejecución
        //- Cada ejecución debe tener el siguiente formato de nombre:
            //- {nombreRepo}-{rama}-{numeroEjecucion} ejemplo:
            //- ms-iclab-feature-estadomundial(Si está usando el CRUD ms-iclab-feature-[nombre de su crud])

            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh "echo 'SonarQube'"
                    //sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=githubfull'
                }
            }
        }
        stage("5: Nexus Upload"){
        //- Subir el artefacto creado al repositorio privado de Nexus.
        //- Ejecutar este paso solo si los pasos anteriores se ejecutan de manera correcta.
            steps {
                script {
                sh "echo 'Nexus Upload'"
                }
            }
        }
        stage("6: gitCreateRelease"){
        //- Crear rama release cuando todos los stages anteriores estén correctamente ejecutados.
        //- Este stage sólo debe estar disponible para la rama develop.
            steps {
                script {
                sh "echo 'gitCreateRelease'"
                }
            }
        }
        ///////Despliegue continuo
        stage("7: gitDiff"){
            //- Mostrar por pantalla las diferencias entre la rama release en curso y la rama
            //master.(Opcional)
            steps {
                script {
                sh "echo 'gitDiff'"
                }
            }
        }
        stage("8: nexusDownload"){
            //- Descargar el artefacto creado al workspace de la ejecución del pipeline.
            steps {
                script {
                sh "echo 'nexusDownload'"
                }
            }
        }
        stage("9: run"){
            //- Ejecutar artefacto descargado.
            steps {
                script {
                sh "echo 'run'"
                //sh 'mvn spring-boot:run &'
                }
            }
        }
        stage("9: test"){
            //- Realizar llamado a microservicio expuesto en local para cada uno de sus
            //métodos y mostrar los resultados.
            steps {
                script {
                sh "echo 'Test'"
                //sh 'sleep 60'
                //sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'
                }
            }
        }
        stage("9: gitMergeMaster"){
            //- Realizar merge directo hacia la rama master.
            //- Ejecutar sólo si todo lo demás resulta de forma exitosa.

            steps {
                script {
                sh "echo 'gitMergeMaster'"
                }
            }
        }
        stage("10: gitMergeDevelop"){
            //- Realizar merge directo hacia rama develop.
            //- Ejecutar sólo si todo lo demás resulta de forma exitosa
            steps {
                script {
                sh "echo 'gitMergeDevelop'"
                }
            }
        }
        stage("11: gitTagMaster"){
            //- Crear tag de rama release en rama master.
            //- Ejecutar sólo si todo lo demás resulta de forma exitosa.
            steps {
                script {
                sh "echo 'gitTagMaster'"
                }
            }
        }
    }
    post {
        always {
            sh "echo 'Siempre se ejecuta'"
        }
        success {
            sh "echo 'Solo en caso de exito'"
        }
        failure {
            sh "echo 'Solo en caso de error'"
        }
    }
}