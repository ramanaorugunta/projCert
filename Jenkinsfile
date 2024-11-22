pipeline {
  agent {
    label 'slave1'
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerloginid')
  }
  stages {
        stage('Puppet Agent Installation') {

        steps {
            sh '''wget https://apt.puppetlabs.com/puppet6-release-focal.deb
                    sudo dpkg -i puppet6-release-focal.deb
                    sudo apt-get update -y
                    sudo apt-get install puppet-agent -y'''
            }
        }   

        stage('SCM_Checkout') {
            steps {
            echo 'Perform SCM_Checkout'
            git 'https://github.com/ramanaorugunta/projCert.git'
            }
        }  

        stage('Build Docker Image') {
            steps {
                sh 'docker version'
                sh "docker build -t oruguntaramana/dcp-sep21-phpwebapp:${BUILD_NUMBER} ."
                sh 'docker image list'
                sh "docker tag oruguntaramana/dcp-sep21-phpwebapp:${BUILD_NUMBER} oruguntaramana/dcp-sep21-phpwebapp:latest"
                }
        }
        stage('Login2DockerHub') {

            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Publish_to_Docker_Registry and Running container"') {
        steps {
            sh "docker push oruguntaramana/dcp-sep21-phpwebapp:latest"
            sh "docker run -it -p 8089:8080 oruguntaramana/dcp-sep21-phpwebapp"
            }
        }    
  }
}