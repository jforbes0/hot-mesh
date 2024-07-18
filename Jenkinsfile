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
                // Add any test steps if necessary
                echo 'No tests available'
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    // Define the production server and deployment path
                    def prodServer = 'jessicaforbes@192.168.1.21'
                    def deployPath = '/home/jessicaforbes/deploy'

                    // Create the deploy path if it doesn't exist
                    sh "ssh ${prodServer} 'mkdir -p ${deployPath}'"

                    // Rsync the files to the production server
                    sh "rsync -avz --delete-after . ${prodServer}:${deployPath}"

                    // SSH into the production server and start the application
                    sh """
                    ssh ${prodServer} '
                    cd ${deployPath} &&
                    pip install -r requirements.txt &&
                    nohup python app.py > app.log 2>&1 &
                    '
                    """
                }
            }
        }
    }
}
