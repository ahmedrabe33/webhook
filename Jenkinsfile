pipeline {
    agent { label 'docker' }  // Run only on Jenkins agents with label "docker"

    environment {
        GITHUB_TOKEN = credentials('github-token') // Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repo using token (if needed for private repo)
                git url: 'https://github.com/ahmedrabe33/webhook.git', credentialsId: 'github-token'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing PHP dependencies...'
                sh '''
                    apt update
                    apt install -y php-cli php-mbstring php-xml php-curl unzip
                    wget -O /usr/local/bin/phpunit https://phar.phpunit.de/phpunit-9.phar
                    chmod +x /usr/local/bin/phpunit
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running PHPUnit tests...'
                sh '''
                    if [ -f "tests/TestExample.php" ]; then
                        phpunit --colors=always tests
                    else
                        echo "No tests found, skipping..."
                    fi
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
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}