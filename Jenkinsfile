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
                    docker.withRegistry('https://hub.docker.com/repository/docker/sanu28221/dev_branch/general', 'DEV_DOC_CREADS') {
                        app.push()
                    }
                }
                echo 'Image Pushed to DEV'
            }
        }
        stage('Pull, Tag, and Push Docker Image to QA') {
            when {
                branch 'QA'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DEV_DOC_CREADS') {
                        docker.image("sanu28221/dev_branch:latest").pull()
                    }
                    echo 'Image pulled from DEV Branch'
                    sh 'docker tag sanu28221/dev_branch:latest sanu28221/qa_branch:latest'
                    docker.withRegistry('https://registry.hub.docker.com', 'QA_DOC_CREADS') {
                        docker.image("sanu28221/qa_branch:latest").push()
                    }
                }
                echo 'Image Pushed to QA'
            }
        }
    }
    post {
        always {
            echo 'Deleting Project now !! '
            deleteDir() // This deletes the entire workspace directory, use with caution.
        }
    }
}
