pipeline {
    agent { label 'docker' }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                echo 'Checking PHP and PHPUnit installation...'
                sh '''
                    php -v
                    phpunit --version
                '''
            }
        }

        stage('Check GitHub Secret') {
            steps {
                echo 'Checking GitHub token from Jenkins Credentials...'
                withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        echo "GitHub token exists securely in Jenkins."
                        test -n "$GITHUB_TOKEN"
                    '''
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running PHPUnit tests...'
                sh '''
                    phpunit --colors=always tests
                '''
            }
        }
    }

    post {
        success {
            echo 'Build succeeded! ✅'
        }

        failure {
            echo 'Build failed! ❌'
        }

        always {
            echo 'Pipeline finished.'
        }
    }
}
