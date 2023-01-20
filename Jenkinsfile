pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yassineakermi/employee_management_api.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t tounsi7orra/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u tounsi7orra -p ${dockerhubpwd}'

}
                   sh 'docker push tounsi7orra/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                  sh 'kubectl  apply -f ./deploymentservice.yaml'
                }
            }
        }
    }
}
