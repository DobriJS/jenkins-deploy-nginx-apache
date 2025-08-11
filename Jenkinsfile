pipeline {
    agent any
    
    stages {
        stage('Checkout source code') {
            steps {
                git url: 'https://github.com/DobriJS/jenkins-deploy-nginx-apache.git', branch: 'main'
            }
        }

        stage('Prepare application directory') {
            steps {
                echo "Preparing ${pwd()}/app-web..."
                sh "rm -rf ${pwd()}/app-web"
                sh "mkdir -p ${pwd()}/app-web"
                sh "cp -r web/* ${pwd()}/app-web/"
                sh "chmod -R a+rX ${pwd()}/app-web"
                sh "ls -l ${pwd()}/app-web"  // Verify files exist
            }
        }

        stage('Drop existing containers') {
            steps {
                sh "docker rm -f app-web-apache || true"
                sh "docker rm -f app-web-nginx || true"
            }
        }

        stage('Create containers in parallel') {
            parallel {
                stage('Apache') {
                    steps {
                        sh """
                        docker run -dit --name app-web-apache \
                        -p 9100:80 \
                        -v ${pwd()}/app-web:/usr/local/apache2/htdocs:ro \
                        httpd
                        """
                    }
                }
                stage('Nginx') {
                    steps {
                        sh """
                        docker run -dit --name app-web-nginx \
                        -p 9200:80 \
                        -v ${pwd()}/app-web:/usr/share/nginx/html:ro \
                        nginx
                        """
                    }
                }
            }
        }
    }
}

