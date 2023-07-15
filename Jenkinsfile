pipeline {
environment {
DOCKERHUB_USERNAME = "yoniss"
APP_NAME = "spring-expense"
}
    agent any
    stages{
  
        stage('Updating kubernetes deployment file') {
        steps{
        script {
                sh """
                cat expense-spring-deployment.yaml | grep image
                sed -i  's|image: .*|image: ${DOCKERHUB_USERNAME}/${APP_NAME}:${IMAGE_TAG}|' expense-spring-deployment.yaml
                cat expense-spring-deployment.yaml | grep image
                """

        }
        }
        }

        stages {
        stage("Clone Git Repository") {
            steps {
                 withCredentials([gitUsernamePassword(credentialsId: 'GITHUB_TOKEN', gitToolName: 'Default')]) {
                                       sh 'git clone -b main https://github.com/yonissam/expense-springboot-argo.git'
                                   }
            }
        }
    }

        stage('Push the changed deployment file to GITHUB') {
        steps{
           script{
                sh """
                   git config --global user.name "yonissam"
                   git config --global user.email "ysisay@icloud.com"
                   git add expense-spring-deployment.yaml
                   git commit -m "updated the deployment file"
                   """
                   withCredentials([gitUsernamePassword(credentialsId: 'GITHUB_TOKEN', gitToolName: 'Default')]) {
                                       sh 'git push https://github.com/yonissam/expense-springboot-argo.git main'
                                   }
           }
        }
        }

    }
}



