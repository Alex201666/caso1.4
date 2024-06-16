pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                // Obtener el código de la rama develop del repositorio
                git branch: 'develop', url: 'https://github.com/Alex201666/caso1.4.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Ey, esto es python y no hay que compilar nada'
                echo WORKSPACE
                sh 'ls -l'
            }
        }
    }
}    
