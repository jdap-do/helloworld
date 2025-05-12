pipeline {
    agent none
    
        stages {
            stage('Get Code') {
                agent { label 'master' }  // agente 1
                steps {
                    echo 'FASE CLONADO============================================================================================='
                    echo 'Clonando código'
                    git 'https://github.com/jdap-do/helloworld.git'
                    bat 'whoami'
                    bat 'hostname'
                    bat 'echo %WORKSPACE%'
                }
            }


            stage('Build') {
                agent { label 'master' }  // agente 2 (en este caso mismo host)
                steps {
                    echo 'FASE BUILD=================================================================================================================='
                    echo 'No hay compilación necesaria ya que es python'
                    bat 'whoami'
                    bat 'hostname'
                    bat 'echo %WORKSPACE%'
                }
            }

            stage('Test') {
                agent { label 'master' }  // agente 3 (simulado)
                steps {
                    echo 'FASE TEST=================================================================================================================='
                    echo 'Ejecutando pruebas con pytest'
                    bat 'whoami'
                    bat 'hostname'
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
            junit 'test-reports/results.xml'
        }
    }
}
