pipeline {
    agent {
        label 'tooling'
    }

    triggers {
        githubPush()
    }

    environment {
        // SERVICE_NAME = "pids-webapp"
        // General variables
        ACCOUNT_ID = "398692602192"
        REGION = "ap-southeast-1"
        DEVOP_ORG = "pids-devops"
        GITHUB_CREDENTIALS = credentials('github')

        // Service variables
        SERVICE_NAME = "webapp"
        ECR_URI = "${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com"
        IMAGE_REPO = "${ECR_URI}/${SERVICE_NAME}"
        // DEVOPS_REPO = "https://github.com/${DEVOP_ORG}/${SERVICE_NAME}"
        IMAGE_TAG = "${BUILD_ID}"
    }

    stages {
        stage('Preparation') {
            steps {
                echo "Login to ECR.."
                echo "${WORKSPACE}"
                sh '''
                    set +x
                    REPO_LOGIN_PWD=$(aws ecr get-login-password --region ap-southeast-1)
                    docker login -u AWS -p $REPO_LOGIN_PWD $ECR_URI
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo No build required for Webapp.
                '''
            }
        }

        stage('Build and Deploy Image ') {
            steps {
                sh '''
                    docker build -t $IMAGE_REPO:$IMAGE_TAG .
                    docker push $IMAGE_REPO:$IMAGE_TAG
                '''
                echo 'Finished pushing image to ECR repo'
            }
        }

        stage('Clone Devops Repo') {
            steps {
                git credentialsId: 'github', url: "https://github.com/${DEVOP_ORG}/${SERVICE_NAME}"

                sh '''
                    envsubst < $WORKSPACE/deploy.tpl > $WORKSPACE/deploy.yml
                    cat $WORKSPACE/deploy.yml
                '''
            }
        }

        stage('Clean App Workspace') {
            steps {
                cleanWs()
            }
        }
    }
}
