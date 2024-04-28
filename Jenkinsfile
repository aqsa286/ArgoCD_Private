pipeline {
    agent any
    environment {
        ECR = '980136594430.dkr.ecr.eu-west-1.amazonaws.com'
        ECR_Name = 'myapp333'
        docker_tag = '1.6'
    }
    stages {
        stage("Git Clone") {
            steps {
                deleteDir()
                git credentialsId: 'YOUR_GITHUB_ACCESS', url: 'https://github.com/aqsa286/ArgoCD_Private.git', branch: 'main'
            }
        }
        stage("Replace Image Tag") {
            steps {
                script {
                    sh """
                    sed -i'' "s|image:.*|image: ${ECR}/${ECR_Name}:${docker_tag}|" deployment.yml
                    """
                }
            }
        }
        stage('Commit changes') {
            steps {
                sh 'git add deployment.yml'
                sh 'git commit -m "Add deployment.yml"'
            }
        }
        stage('Push changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'YOUR_GITHUB_ACCESS', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                    sh "git push https://${GITHUB_USERNAME}:${GITHUB_TOKEN}@github.com/aqsa286/ArgoCD_Private.git main"
                }
            }
        }
    }
}
