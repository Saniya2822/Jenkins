pipeline {
    agent any

    stages {
        stage('Build Docker Dev Image') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    def app = docker.build("sanu28221/dev_branch:latest")
                    docker.withRegistry('https://registry.hub.docker.com/sanu28221/', 'CREADS') {
                        app.push()
                    }
                }
            }
        }

        stage('Build Docker QA Image') {
            when {
                branch 'qa'
            }
            steps {
                script {
                    def app = docker.build("sanu28221/dev_branch:latest")
                    docker.withRegistry('https://registry.hub.docker.com/sanu28221/', 'CREADS') {
                        app.push()
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Deleting Project now !! '
            deleteDir()
        }
    }
}