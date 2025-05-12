pipeline {
    agent any
    
    stages {
        stage('Get Code') {
            steps {
                echo 'Clonando codigo'
                //Obtener código desde mi repositorio, ya que hice un fork
                git 'https://github.com/jdap-do/helloworld.git'
                bat 'dir'
                bat 'echo %WORKSPACE%' 
            }
        }
        stage('Etapa Build') {
            steps {
                echo 'NO HAY QUE COMPILAR NADA, ESTO ES PYTHON'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas unitarias con pytest'
                bat '''
                    mkdir test-reports
                    set PYTHONPATH=.
                    "C:\\Users\\joeda\\AppData\\Local\\Programs\\Python\\Python313\\Scripts\\pytest.exe" test/unit --junitxml=test-reports/results.xml || exit /b %errorlevel%
                '''
            }
        }
    }

    post {
        always {
            junit 'test-reports/results.xml'
        }
    }
}