pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent { label 'agent-clone' }
            steps {
                echo 'FASE CLONADO=================================================================================================================='
                git 'https://github.com/jdap-do/helloworld.git'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Build') {
            agent { label 'agent-build' }
            steps {
                echo 'FASE BUILD=================================================================================================================='
                git 'https://github.com/jdap-do/helloworld.git'
                echo 'No hay compilación necesaria ya que es Python'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Test') {
            agent { label 'agent-test' }
            steps {
                echo 'FASE TEST=================================================================================================================='
                git 'https://github.com/jdap-do/helloworld.git'
                echo 'Levantando microservicio Flask y mock WireMock'

                // Lanzar Flask app
                bat '''
                    start /B cmd /c "set FLASK_APP=app/main.py && set FLASK_RUN_PORT=5000 && flask run"
                '''

                // Lanzar WireMock
                bat '''
                    start /B cmd /c "java -jar wiremock-standalone-2.35.0.jar --port 9090"
                '''

                echo 'Esperando que servicios arranquen...'
                bat 'timeout /t 5'

                echo 'Ejecutando pruebas con pytest'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
                bat '''
                    mkdir test-reports
                    set PYTHONPATH=.
                    "C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" test/rest --junitxml=test-reports/results.xml || exit /b 1
                '''
            }
        }
    }

    post {
        always {
            node('agent-test') {
                junit 'test-reports/results.xml'
            }
        }
    }
}
