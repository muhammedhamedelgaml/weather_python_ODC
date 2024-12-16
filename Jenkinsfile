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
                     sh ' ansible -i ansible/inventory vms --private-key=/var/lib/jenkins/.ssh/id_ODC -m ping '
                  // sh ' ansible-playbook -i ansible/inventory  ansible/playbook.yml '
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
