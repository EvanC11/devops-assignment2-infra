pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'v1.0.2', description: 'Docker image tag to deploy')
    }

    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    ansible-playbook playbooks/deploy_app.yml -e "docker_image_tag=${IMAGE_TAG}"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    ansible-playbook playbooks/verify_app.yml
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment succeeded for image tag: ${IMAGE_TAG}"
        }
        failure {
            echo "Deployment failed for image tag: ${IMAGE_TAG}"
        }
    }
}