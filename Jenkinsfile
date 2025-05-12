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
