pipeline {
    agent any

    stages {
        stage('Prepare directory for the WEB Application') {
            steps {
                sh "rm -rf ${pwd()}/app-web"
                sh "mkdir ${pwd()}/app-web"
            }
        }

        stage('Drop the containers') {
            steps {
                echo 'Dropping the containers...'
                sh 'docker rm -f app-web-apache || true'
                sh 'docker rm -f app-web-nginx || true'
            }
        }

         stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'
                sh "cp -r web/* ${pwd()}/app-web"
            }
        }

        stage('Create the containers in Parallel') {
            parallel {
                stage('Create the Apache container') {
                    steps {
                        echo 'Creating the Apache Container...'
                        sh "docker run -dit --name app-web-apache -p 9100:80 -v ${pwd()}/app-web:/usr/local/apache2/htdocs/ httpd"
                    }
                }
                stage('Create the Nginx container') {
                    steps {
                        echo 'Creating the Nginx container...'
                        sh "docker run -dit --name app-web-nginx -p 9200:80 -v ${pwd()}/app-web:/usr/share/nginx/html nginx"
                    }
                }
            }
        }

        stage('Wait for the containers to be ready') {
            steps {
                echo 'Waiting for the containers to be ready...'
                sh 'sleep 5'
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'
                sh "cp -r web/* ${pwd()}/app-web"
            }
        }
    }

    post {
        success {
            echo 'The deployment in Nginx and Apache has worked'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'web/*', followSymlinks: false
            cleanWs()
        }
        failure {
            echo 'An error has occurred in the deploy'
        }
    }
}
