node {
    environment {
        // Ruta al ejecutable de Python
        PYTHON_PATH = 'C:\\Users\\EQUIPO\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
        // Ruta al script de envío de correo
        EMAIL_SCRIPT_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\scripts\\send_email.py'
    }

    stages {
        stage('Revisión') {
            steps {
                // Checkout del código fuente desde el repositorio bifurcado
                git url: 'https://github.com/tu_usuario/tu_repositorio_forked.git', branch: 'master'
            }
        }

        stage('Instalación de dependencias') {
            steps {
                bat 'npm install'
            }
        }

        stage('Construir') {
            steps {
                bat 'npm run ng build'
            }
        }

        stage('Limpiar') {
            steps {
                bat 'rd /s /q C:\\servidor\\fire'
            }
        }

        stage('Mover al servidor') {
            steps {
                bat 'xcopy C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\angular-pipeline\\dist\\app-03\\browser C:\\servidor\\fire /E /I /Y'
            }
        }
    }

    post {
        always {
            // Esta etapa se ejecutará siempre, independientemente del estado del pipeline
            echo 'Pipeline completo, enviando correo...'
            bat "${PYTHON_PATH} ${EMAIL_SCRIPT_PATH}"
        }
        success {
            echo 'Construcción, despliegue y envío de correo exitosos!'
        }
        failure {
            echo 'Construcción, despliegue o envío de correo fallidos!'
        }

}
