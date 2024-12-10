pipeline {
    agent any
    environment {
        registry = "muhammedhamedelgaml/app_python"
        dockerhubCreds = credentials('dockerhub')  
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
                        echo $PASSWORD | docker login --username $USERNAME --password-stdin
                        # docker login --username $USERNAME --password $PASSWORD
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
