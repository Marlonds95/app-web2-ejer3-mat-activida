node {
    // Define las variables de entorno
    def pythonPath = 'C:\\Users\\EQUIPO\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
    def emailScriptPath = 'C:\\ProgramData\\Jenkins\\.jenkins\\scripts\\send_email.py'

    try {
        stage('Revisión') {
            // Checkout del código fuente desde el repositorio bifurcado
            checkout([
                $class: 'GitSCM',
                branches: [[name: '*/master']],
                userRemoteConfigs: [[url: 'https://github.com/Marlonds95/app-web2-ejer3-mat-activida.git']]
            ])
        }

        stage('Instalación de dependencias') {
            // Instala las dependencias del proyecto
            bat 'npm install'
        }

        stage('Construir') {
            // Construye el proyecto Angular
            bat 'npm run ng build'
        }

        stage('Limpiar') {
            // Limpia la carpeta en el servidor
            bat 'rd /s /q C:\\servidor\\fire'
        }

        stage('Mover al servidor') {
            // Copia los archivos construidos al servidor
            bat 'xcopy C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\actividad-pipe-angular\\dist\\app-03\\browser C:\\servidor\\fire /E /I /Y'
        }

    } catch (Exception e) {
        // Manejo de errores en caso de que algo falle en las etapas
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        stage('Enviar correo') {
            // Ejecuta el script de Python para enviar un correo electrónico
            bat "${pythonPath} ${emailScriptPath}"
        }
    }
}

