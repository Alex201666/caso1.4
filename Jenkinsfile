pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                // Obtener el código de la rama develop del repositorio
                git branch: 'develop', url: 'https://github.com/Alex201666/caso1.4.git'
            }
        }

        stage('Static'){
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                    sh '''
                        rm flake8.out
                        sudo python3 -m flake8 --exit-zero --format=pylint app >flake8.out
                    '''
                    recordIssues tools: [flake8(name: 'Flake8', pattern: 'flake8.out')]
                    }        
                }
        }
        stage('Security'){
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                     sh '''
                        rm bandit.out
                        sudo python3 -m bandit -r . -f custom -o bandit.out -ll -ii --msg-template "{abspath}:{line}: [{test_id}] {msg}"
                    '''
                    recordIssues tools: [pyLint(name: 'Bandit', pattern: 'bandit.out')]
                    }
                }
        }
    }
}    






        
