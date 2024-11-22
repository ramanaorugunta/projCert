pipeline {
  agent none
  stages{
    stage ('Puppet Agent Installation') {
      agent {
         label 'slave'
        }
      steps {
        sh '''wget https://apt.puppetlabs.com/puppet6-release-focal.deb
              sudo dpkg -i puppet6-release-focal.deb
              sudo apt-get update -y
              sudo apt-get install puppet-agent -y'''
        }
    }
   stage ('Docker installation'){
     agent any
     steps{
        git 'https://github.com/Sudhey/DevopsCertification.git'
        dir('.') {
          sh ' sudo ansible-playbook playbook.yaml -i inventory.txt'
        }
    }       
   }
   stage("Building image and running container"){
    agent any 
    steps{
        git 'https://github.com/Sudhey/DevopsCertification.git'
        //script {
        //    error ( ' Intentional failure')
        //}
        sshPublisher(publishers: [sshPublisherDesc(configName: 'Test Server', transfers: [
            sshTransfer(
                cleanRemote: false, 
                excludes: '', 
                execCommand: '''
                    docker container stop edureka_demo
                    docker container rm -f edureka_demo
                    docker image rmi -f edureka_demo
                    cd /home/ubuntu/docker
                    docker image build -t edureka_demo .
                ''',
                execTimeout: 120000, 
                flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '/docker', 
                remoteDirectorySDF: false, 
                removePrefix: '', 
                sourceFiles: '**/*'
            ),
            sshTransfer(
                cleanRemote: false, 
                excludes: '', 
                execCommand: 'docker container run -dit --name edureka_demo -p 80:80 edureka_demo', 
                execTimeout: 120000, 
                flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: '', 
                
                sourceFiles: ''
            )
        ], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
   }
    stage("Delete Container"){
        agent any
        when {
            expression {
                currentBuild.result == 'FAILURE' ||
                currentBuild.result == 'UNSTABLE'
            }
        }
        steps{
            sshPublisher(publishers: [sshPublisherDesc(configName: 'Test Server', transfers: [
            sshTransfer(
                cleanRemote: false, 
                excludes: '', 
                execCommand: '''
                    docker container stop edureka_demo
                    docker container rm -f edureka_demo
                ''', 
                execTimeout: 120000, 
                flatten: false, 
                makeEmptyDirs: false, 
                noDefaultExcludes: false, 
                patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: '', 
                sourceFiles: ''
            )
        ], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }
    }
}
}