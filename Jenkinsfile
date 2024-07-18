pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    // Define the production server and deployment path
                    def prodServer = ‘jessicaforbes@192.168.1.21’
                    def deployPath = '/home/jessicaforbes/hot-mesh'

                    // Run deployment commands
                    sh """
                    rsync -avz --delete-after . ${prodServer}:${deployPath}
                    ssh ${prodServer} 'cd ${deployPath} && ./deploy.sh'
                    """
                }
            }
        }
    }
}
