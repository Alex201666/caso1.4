pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                //Obtener el código del repositorio
                git 'https://github.com/Alex201666/caso1.4.git'
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