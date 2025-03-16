pipeline {
    agent any

    parameters {
        string(name: 'LOGIN', defaultValue: '', description: 'Login del usuario (Nombre + Apellido)')
        string(name: 'NAME', defaultValue: '', description: 'Nombre completo del usuario')
        string(name: 'DEPARTMENT', defaultValue: '', description: 'Departamento del usuario')
    }

    environment {
        PASSWORD = sh(script: 'openssl rand -base64 12', returnStdout: true).trim()  // Generar contraseña aleatoria
        EMAIL_RECIPIENT = "${params.LOGIN}@nanotech.com"  // Correo basado en el login
    }

    stages {
       stage('Create User') {
    steps {
        script {
            sh """
            sudo groupadd -f ${params.DEPARTMENT}  
            sudo useradd -m -g ${params.DEPARTMENT} -s /bin/bash ${params.LOGIN}
            echo "${params.LOGIN}:${env.PASSWORD}" | sudo chpasswd
            sudo chage -d 0 ${params.LOGIN}  # Obliga al usuario a cambiar la contraseña en el primer inicio de sesión
            """
        }
    }
}
        stage('Send Email') {
            steps {
                script {
                    sh """
                    echo "Hola ${params.NAME},\n\nTu cuenta ha sido creada. Tu contraseña temporal es: ${env.PASSWORD}\n\nPor favor, cámbiala en tu primer inicio de sesión."
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Fin del pipeline!'
        }
    }
}
