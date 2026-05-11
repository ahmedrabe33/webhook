pipeline {
    agent { label 'docker' }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from public GitHub repository...'
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                echo 'Verifying PHP and PHPUnit...'
                sh '''
                    php -v
                    phpunit --version
                '''
            }
        }

        stage('Show Workspace') {
            steps {
                echo 'Showing project files...'
                sh '''
                    pwd
                    ls -la
                    echo "Tests directory:"
                    ls -la tests || true
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running PHPUnit tests...'
                sh '''
                    if [ -d "tests" ]; then
                        phpunit --colors=always tests
                    else
                        echo "ERROR: tests directory not found"
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo 'Build succeeded! Unit tests passed ✅'
        }

        failure {
            echo 'Build failed! Check the console output ❌'
        }

        always {
            echo 'Pipeline finished.'
        }
    }
}
#1