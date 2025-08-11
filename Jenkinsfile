pipeline {
    agent any
    
    stages {
        stage('Prepare application directory') {        
            steps {
                echo 'Cleaning and creating app-web directory...'
                sh "rm -rf ${pwd()}/app-web"
                sh "mkdir ${pwd()}/app-web"

                echo 'Copying web application files before container creation...'
                sh "cp -r web/* ${pwd()}/app-web"
                sh "chmod -R a+rX ${pwd()}/app-web"
            }
        }

        stage('Drop existing containers') {   
            steps {
                echo 'Dropping existing containers (if any)...'
                sh 'docker rm -f app-web-apache || true'
                sh 'docker rm -f app-web-nginx || true'
            }
        }

        stage('Create containers in parallel') {
            parallel {
                stage('Apache container') {           
                    steps {
                        echo 'Starting Apache container...'
                        sh """
                        docker run -dit --name app-web-apache \
                        -p 9100:80 \
                        -v ${pwd()}/app-web:/usr/local/apache2/htdocs/ \
                        httpd
                        """
                    }
                }
                stage('Nginx container') {
                    steps {
                        echo 'Starting Nginx container...'
                        sh """
                        docker run -dit --name app-web-nginx \
                        -p 9200:80 \
                        -v ${pwd()}/app-web:/usr/share/nginx/html \
                        nginx
                        """
                    }
                }       
            }   
        }
    }

    post {              
        success {
            echo 'Deployment to Nginx and Apache succeeded.'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'web/*', followSymlinks: false
            cleanWs()         
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

