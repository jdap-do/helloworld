pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent { label 'agent-clone' }
            steps {
                echo 'FASE CLONADO=================================================================================================================='
                echo 'Clonando código'
                git 'https://github.com/jdap-do/helloworld.git'
                echo 'whoami'
                bat 'whoami'
                echo 'hostname'
                bat 'hostname'
                echo 'echo %WORKSPACE%'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Build') {
            agent { label 'agent-build' }
            steps {
                echo 'FASE BUILD=================================================================================================================='
                echo 'Clonando código'
                git 'https://github.com/jdap-do/helloworld.git'
                echo 'No hay compilación necesaria ya que es python'
                echo 'whoami'
                bat 'whoami'
                echo 'hostname'
                bat 'hostname'
                echo 'echo %WORKSPACE%'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Test') {
            agent { label 'agent-test' }
            steps {
                echo 'FASE TEST=================================================================================================================='
                echo 'Clonando código'
                git 'https://github.com/jdap-do/helloworld.git'
                echo 'Ejecutando pruebas con pytest'
                echo 'whoami'
                bat 'whoami'
                echo 'hostname'
                bat 'hostname'
                echo 'echo %WORKSPACE%'
                bat 'echo %WORKSPACE%'
                bat '''
                    mkdir test-reports
                    set PYTHONPATH=.
                    "C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" test/unit --junitxml=test-reports/results.xml || exit /b 1
                '''
            }
        }
    }

    post {
        always {
            node('agent-test') {
                // Lanzar WireMock en segundo plano
                bat '''
                    start "" /B java -jar wiremock-jre8-standalone-2.28.0.jar --port 9090 --root-dir wiremock
                    timeout /t 3
                '''

                // Ejecutar tests de integración
                bat '''
                    mkdir test-reports
                    set PYTHONPATH=.
                    "C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" test/rest --junitxml=test-reports/results.xml || exit /b 1
                '''

                // Publicar resultados
                junit 'test-reports/results.xml'
            }
        }
    }
}
