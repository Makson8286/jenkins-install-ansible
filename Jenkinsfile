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
                cd /your-path/
                git clone https://github.com/Makson8286/jenkins-install-ansible      
                '''
            }
        }    
        stage("Build") {
            steps {
                sh '''
                cd /your-path/jenkins-install-ansible/Ansible
                docker build -t makson8286/ansible .
                '''
            }
        } 
        stage("Postgres") {
            steps {
                sh '''
                docker run \
                --name ansible \
                -d makson8286/ansible
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
                docker push makson8286/ansible
                '''
            }
        }
    }
}
