pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent { label 'agent-clone' }
            steps {
                checkout scm
                withEnv(["PATH+PYTHON=C:\\Python313\\Scripts"]) {
                    echo "Clonando codigo"
                    git url: 'https://github.com/jdap-do/helloworld.git'
                    bat 'echo %WORKSPACE%'
                }
            }
        }

        stage('Etapa Build') {
            agent { label 'agent-build' }
            steps {
                echo 'NO HAY QUE COMPILAR NADA, ESTO ES PYTHON'
            }
        }

        stage('Test') {
            agent { label 'agent-test' }
            steps {
                withEnv(["PATH+PYTHON=C:\\Python313\\Scripts"]) {
                    echo 'Ejecutando pruebas con pytest'
                    bat 'mkdir test-reports'
                    bat 'set PYTHONPATH=.'

                    //  Lanza el servidor flask y wiremock (esto sobrecarga en master)
                    bat 'start "" /B python app/api.py'
                    bat 'start "" /B java -jar wiremock-jre8-standalone-2.28.0.jar --port 9090 --root-dir wiremock'
                    bat 'timeout /t 5'

                    // Ejecutar todos los tests (unitarios + de integración)
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
