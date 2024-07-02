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
                        if [ -f flake8.out ]; then
                            rm flake8.out
                        fi
                        python3 -m flake8 --exit-zero --format=pylint app >flake8.out
                    '''
                    recordIssues tools: [flake8(name: 'Flake8', pattern: 'flake8.out')]
                    }        
                }
        }
        stage('Security'){
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                     sh '''
                        if [ -f bandit.out ]; then
                            rm bandit.out
                        fi
                        python3 -m bandit -r . -f custom -o bandit.out -ll -ii --msg-template "{abspath}:{line}: [{test_id}] {msg}"
                    '''
                    recordIssues tools: [pyLint(name: 'Bandit', pattern: 'bandit.out')]
                    }
                }
        }

        stage('Deploy'){
            steps {
                withAWS(region: "${AWS_DEFAULT_REGION}") {
                    sh """
                            sam deploy \
                                --stack-name todo-list-aws-staging \
                                --s3-bucket bucket-tarea1d \
                                --s3-prefix todo-list-aws \
                                --region us-east-1 \
                                --capabilities CAPABILITY_IAM \
                                --parameter-overrides Stage=staging \
                                --no-fail-on-empty-changeset
                        """
                }        
            }
        }

        stage('Rest Test') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    script {
                        def BASE_URL = sh(script: """
                            aws cloudformation describe-stacks --stack-name todo-list-aws-production --query "Stacks[0].Outputs[?OutputKey=='BaseUrlApi'].OutputValue" --output text --region us-east-1
                        """, returnStdout: true).trim()
                
                        echo "Using Base URL: ${BASE_URL}"
                
                        withEnv(["BASE_URL=${BASE_URL}"]) {
                            sh 'python3 -m pytest --junitxml=report.xml test/integration/todoApiTest.py --disable-warnings'
                        }
                    }
                }
            }
    }
}    

