pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent { label 'agent-clone' }
            steps {
                checkout scm
                echo "FASE CLONADO=================================================================================================================="
                git url: 'https://github.com/jdap-do/helloworld.git'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Build') {
            agent { label 'agent-build' }
            steps {
                checkout scm
                echo "FASE BUILD=================================================================================================================="
                git url: 'https://github.com/jdap-do/helloworld.git'
                echo "No hay compilación necesaria ya que es python"
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Test') {
            agent { label 'agent-test' }
            steps {
                checkout scm
                echo "FASE TEST=================================================================================================================="
                git url: 'https://github.com/jdap-do/helloworld.git'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'

                // Lanzamos Flask y WireMock sin esperar
                bat 'start "" /B python app/api.py'
                bat 'start "" /B java -jar wiremock-jre8-standalone-2.28.0.jar --port 9090 --root-dir wiremock'

                // Ejecutamos las pruebas inmediatamente
                bat 'mkdir test-reports'
                bat 'set PYTHONPATH=. && "C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" test/rest --junitxml=test-reports/results.xml || exit /b 1'
            }
        }
    }

    post {
        always {
            node('agent-test') {
                echo "Ejecutando reporte de pruebas..."
                junit 'test-reports/results.xml'
            }
        }
    }
}
