pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/BhushanShetty/sample-2'
            }
        }

        stage('Install') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest --maxfail=1 --disable-warnings -q
                '''
            }
        }

        stage('Trivy Scan') {
            steps {
                echo 'Running Trivy vulnerability scan...'
                sh '''
                    mkdir -p trivy-reports
                    trivy fs --format json -o trivy-reports/trivy-report.json .
                '''
            }
            post {
                always {
                    archiveArtifacts artifacts: 'trivy-reports/trivy-report.json', fingerprint: true
                }
            }
        }
    }
}

stage('Trivy Scan') {
    steps {
        sh '''
            trivy image --format json -o trivy-report.json my-image:latest
        '''
    }
}






