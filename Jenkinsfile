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
                    docker.withRegistry('https://index.docker.io/v1/', 'DEV_DOC_CREADS') {
                        docker.image("sanu28221/dev_branch:latest").push()
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
                    docker.image("sanu28221/dev_branch:latest").pull()
                    sh 'docker tag sanu28221/dev_branch:latest sanu28221/qa_branch:latest'
                    docker.withRegistry('https://index.docker.io/v1/', 'QA_DOC_CREADS') {
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
            deleteDir()
        }
    }
}
