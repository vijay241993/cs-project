pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockercreds')
        GIT_REPO_URL = 'https://github.com/N-Moorthy/CapstoneProject.git'
        GIT_CREDENTIALS_ID = 'gitcreds'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'chmod +x build.sh'
                    sh './build.sh'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Clean up previous deployments
                    sh 'docker-compose down'
                    
                    // Ensure deploy.sh is executable and run it with branch name argument
                    sh 'chmod +x deploy.sh'
                    sh "./deploy.sh ${env.GIT_BRANCH}"
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up...'
                sh 'docker logout'
            }
        }
        
        failure {
            script {
                echo 'Deployment failed.'
            }
        }
    }
}
