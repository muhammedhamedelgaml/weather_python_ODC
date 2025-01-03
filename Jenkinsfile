def COLOR_MAP = [
  'SUCCESS' : 'good',
  'FAILURE' : 'danger'
]

pipeline {
    agent any
    environment {
        image_name = "muhammedhamedelgaml/app_python"
    }
    stages {
        stage('Build image') {
            steps {
                script {
                      echo "BUILD DONE"
                      sh ' docker build -t ${image_name}:${BUILD_NUMBER} . '
                }
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {   
                    sh '''
                        docker login --username $USERNAME --password $PASSWORD
                        docker push ${image_name}:${BUILD_NUMBER}

                    '''

                }
            }
        }
              
        stage("Ansible Deploy to vagrant VMs") {
            steps {
                script {
                    ansiblePlaybook(
                        inventory     : 'ansible/inventory',
                        playbook      : 'ansible/site.yml',
                        installation  : 'ansible',  
                        colorized     : false,
                        extraVars     : [
                            IMAGE: "${image_name}",
                            TAG: "${BUILD_NUMBER}"
                        ]
                    )
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished! \n logging out from docker'
            sh 'docker logout'

              
            echo 'slack notification.'
            slackSend channel: '#cicdjenkins',
            color:  COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n for more info visit : ${env.BUILD_URL} " 
        
        }
    }
}
