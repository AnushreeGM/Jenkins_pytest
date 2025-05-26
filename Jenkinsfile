pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/AnushreeGM/Jenkins_pytest.git']])
            }
        }
        stage('Installation') {
            steps {
                sh '''
                sudo apt install -y python3-venv
                sudo apt install -y python3-pytest
                python3 -m venv venv
                . venv/bin/activate
                pip install pytest coverage
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'python3 ops.py'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }
        stage('Generate Code Coverage Report') {
            steps {
                sh '''
                . venv/bin/activate
                coverage run -m pytest
                coverage report
                coverage html -d coverage_html
                '''
            }
        }
        stage('Archive artifacts') {
            steps {
                archiveArtifacts artifacts: 'coverage_html/**', followSymlinks: false
            }
        }
    }
}
