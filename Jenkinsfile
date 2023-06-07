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
                cd /home/st/
                git clone https://github.com/Makson8286/site.jenk         
                '''
            }
        }    
        stage("Build") {
            steps {
                sh '''
                cd /home/st/site.jenk/Site
                docker build -t makson8286/work .
                '''
            }
        } 
        stage("Postgres") {
            steps {
                sh '''
                docker run \
                --name apache2 \
                -p 80:80 \
                -d makson8286/work
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
    }
}
