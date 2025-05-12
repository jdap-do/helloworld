pipeline {
    agent any

    stages {
        stage('Get Code') {
            agent any
            steps {
                echo 'Clonando código'
                git 'https://github.com/jdap-do/helloworld.git'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Build') {
            agent any
            steps {
                echo 'No hay compilación necesaria'
                bat 'whoami'
                bat 'hostname'
                bat 'echo %WORKSPACE%'
            }
        }

        stage('Test') {
            agent any
            steps {
                echo 'Ejecutando pruebas con pytest'
                bat '''
                    whoami
                    hostname
                    echo %WORKSPACE%
                    mkdir test-reports
                    set PYTHONPATH=.
                    "C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" test/unit --junitxml=test-reports/results.xml || exit /b %errorlevel%
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: '**/test-reports/results.xml'
        }
    }
}
