pipeline {
    agent none

    stages {
        stage('Get Code') {
            agent { label 'agent-clone' }
            steps {
                echo "FASE CLONADO=================================================================================================================="
                echo "Clonando código"
                git url: 'https://github.com/jdap-do/helloworld.git'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Build') {
            agent { label 'agent-build' }
            steps {
                echo "FASE BUILD=================================================================================================================="
                echo "Clonando código"
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
                echo "FASE TEST=================================================================================================================="
                echo "Clonando código"
                git url: 'https://github.com/jdap-do/helloworld.git'
                echo "Ejecutando pruebas REST con pytest"
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
                echo "Ejecutando reporte de pruebas..."
                junit 'test-reports/results.xml'
            }
        }
    }
}
