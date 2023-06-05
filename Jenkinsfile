#!groovy
//  groovy Jenkinsfile
properties([disableConcurrentBuilds()])\

pipeline  {
        agent { 
           label ''
        }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
    stages {
        stage("Git clone") {
            steps {
                sh '''
                cd /home/dik/gitr/
                git clone https://github.com/Makson8286/jenk           
                '''
            }
        }    
        stage("Build") {
            steps {
                sh '''
                cd /home/dik/site.jenk/Site
                docker build -t makson8286/sites .
                '''
            }
        } 
        stage("Postgres") {
            steps {
                sh '''
                docker run \
                --name site \
                -p 8080 \
                -d makson8286/zabk:post
                '''
            }
        }
        stage("docker login") {
            steps {
                echo " ============== docker login =================="
                withCredentials([usernamePassword(credentialsId: 'DockerHub-Credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    docker login -u $USERNAME -p $PASSWORD
                    '''
                }
            }
        }
        stage("docker push") {
            steps {
                echo " ============== pushing image =================="
                sh '''
                docker push makson8286/sites
                '''
            }
        }
    }
}
