pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Clone Repository') {
            steps {
                retry(3) {
                    timeout(time: 2, unit: 'MINUTES') {
                        git url: 'https://github.com/fouad-lxix/ClassTask-25-Sept.git', branch: 'main'
                    }
                }
            }
        }
        stage('Set Up Python') {
    steps {
        script {
            if (isUnix()) {
                sh '''
                    python3 -m venv venv || python -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install pytest
                '''
            } else {
                bat '''
                    C:/Users/Fouad/AppData/Local/Microsoft/WindowsApps/python.exe -m venv venv
                    venv/Scripts/activate
                    C:/Users/Fouad/AppData/Local/Microsoft/WindowsApps/python.exe -m pip install --upgrade pip
                    venv/Scripts/pip install pytest
                '''
            }
        }
    }
}

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            . venv/bin/activate
                            python -m unittest discover
                        '''
                    } else {
                        bat '''
                            call venv\\Scripts\\activate.bat
                            python -m unittest discover
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up virtual environment'
            script {
                if (isUnix()) {
                    sh 'deactivate || true'
                } else {
                    bat 'call venv\\Scripts\\deactivate.bat || exit 0'
                }
            }
        }
        success {
            echo 'Build passed!'
        }
        failure {
            echo 'Build failed please update code!'
        }
    }
}