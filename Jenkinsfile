pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent { label 'agent-clone' }
            steps {
                checkout scm
                withEnv(["PATH+PYTHON=C:\\Python313\\Scripts"]) {
                    echo "FASE CLONADO=================================================================================================================="
                    echo "Clonando código"
                    git url: 'https://github.com/jdap-do/helloworld.git'
                    bat 'whoami'
                    bat 'hostname'
                    bat 'echo %WORKSPACE%'
                }
            }
        }

        stage('Build') {
            agent { label 'agent-build' }
            steps {
                checkout scm
                withEnv(["PATH+PYTHON=C:\\Python313\\Scripts"]) {
                    echo "FASE BUILD=================================================================================================================="
                    echo "Clonando código"
                    git url: 'https://github.com/jdap-do/helloworld.git'
                    echo "No hay compilación necesaria ya que es python"
                    bat 'whoami'
                    bat 'hostname'
                    bat 'echo %WORKSPACE%'
                }
            }
        }

        stage('Test') {
            agent { label 'agent-test' }
            steps {
                checkout scm
                withEnv(["PATH+PYTHON=C:\\Python313\\Scripts"]) {
                    echo "FASE TEST=================================================================================================================="
                    echo "Clonando código"
                    git url: 'https://github.com/jdap-do/helloworld.git'
                    echo "Ejecutando pruebas con pytest"
                    bat 'whoami'
                    bat 'hostname'
                    bat 'echo %WORKSPACE%'
                    
                    bat 'mkdir test-reports'
                    bat 'set PYTHONPATH=.'
                    
                    // 👉 Iniciar Flask app y WireMock en segundo plano
                    bat 'start "" /B python app/api.py'
                    bat 'start "" /B java -jar wiremock-jre8-standalone-2.28.0.jar --port 9090 --root-dir wiremock'
                    bat 'timeout /t 5'

                    // 👉 Ejecutar todos los tests
                    bat '"C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" --junitxml=test-reports/results.xml || exit /b 1'
                }
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
