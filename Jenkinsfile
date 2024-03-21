pipeline {
    agent any
    stages {
        stage('Build Docker Image in DEV') {
            when {
                branch 'DEV'
            }
            steps {
                script {
                    def app = docker.build("sanu28221/dev_branch:latest")
                    docker.withRegistry('https://hub.docker.com/repository/docker/sanu28221/', 'DEV_DOC_CREADS') {
                        docker.image("sanu28221/dev_branch:latest").push()
                    }
                }
                sh 'echo Image Pushed to DEV'
            }
        }
        stage('Pull Tag push Docker Image to QA') {
            when {
                branch 'QA'
            }
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/repository/docker/sanu28221/', 'DEV_DOC_CREADS') {
                        docker.image("sanu28221/dev_branch:latest").pull()
                    }
                }
                sh 'echo Image pull from DEV Branch'
                sh 'echo Tagging Docker image from Dev to QA'
                sh "docker tag sanu28221/dev_branch:latest  sanu28221/qa_branch:latest" 
                script {
                    docker.withRegistry('https://hub.docker.com/repository/docker/sanu28221/', 'QA_DOC_CREADS') {
                        docker.image("sanu28221/qa_branch:latest").push()
                    }
                }
                sh 'echo Image Pushed to QA'
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