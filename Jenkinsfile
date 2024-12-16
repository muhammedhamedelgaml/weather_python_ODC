pipeline {
    agent any
    environment {
        registry = "muhammedhamedelgaml/app_python"
    }
    stages {
        stage('Build image') {
            steps {
                script {
                    echo "BUILD DONE"
                    // docker.build("${registry}:v${BUILD_NUMBER}")
                }
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {   
                    sh '''
                        docker login --username $USERNAME --password $PASSWORD     
                    '''
                }
            }
        }

        stage("Ansible Deploy to vagrant VMs") {
            steps {
                script {
                    //  sh ' ansible -i ansible/inventory vms -m ping '
                    //  sh ' ansible-playbook -i ansible/inventory  ansible/playbook.yml '
            ansiblePlaybook(
            inventory     : 'ansible/inventory',
            playbook      :  'ansible/site.yml',
            installation  :  'ansible',
            colorized     :   false ,
                )
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished! \n logout from docker'
            sh 'docker logout'
        }
    }
}
