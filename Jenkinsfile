pipeline {
    agent any
    stages {
        stage('install-pip-deps') {
            steps {
                echo 'Installing python dependencies...'
                // Veikt klonēšanu repozitorijam
                git branch: 'main', url: 'https://github.com/mtararujs/python-greetings.git'
                bat 'dir' //aka ls

                // Izveidot venv un instalēt bibliotēkas
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\python -m pip install -r requirements.txt'

                echo 'Dependencies installed.'
            }
        }
        stage('deploy-to-dev') {
            steps {
                script {
                    deploy("dev", "7001")
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script {
                    test("dev")
                }
            }
        }
        stage('deploy-to-stg') {
            steps {
                script {
                    deploy("stg", "7002")
                }
            }
        }
        stage('tests-on-stg') {
            steps {
                script {
                    test("stg")
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script {
                    deploy("preprod", "7003")
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script {
                    test("preprod")
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script {
                    deploy("prod", "7004")
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script {
                    test("prod")
                }
            }
        }
    }
}


def deploy(String env, String port) {
    echo "Deployment to ${env} environment has started..."

    // Veikt klonēšanu repozitorijam
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    // Delete pm2
    bat "pm2 delete greetings-app-${env} || exit /b 0"
    // Palaist servisu izmantojot pm2
    bat "pm2 start app.py --name greetings-app-${env} --interpreter venv\\Scripts\\python -- --port ${port}"

    echo "Deployment to ${env} environment finished successfully."
}

def test(String env) {
    echo "Testing on ${env} environment has started..."
    
    // Veikt klonēšanu repozitorijam
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    // Izpildīt npm install
    bat 'npm install'
    // Izpildīt npm run
    bat "npm run greetings greetings_${env}"

    echo "Testing on ${env} environment finished successfully."
}