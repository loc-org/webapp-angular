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
        TAG = "${BUILD_ID}"
    }

    stages {
        stage('Preparation') {
            steps {
                echo "Login to ECR.."
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
                    docker build -t $IMAGE_REPO:$TAG .
                    docker push $IMAGE_REPO:$TAG
                '''
                echo 'Finished pushing image to ECR repo'
            }
        }

        stage('Clean App Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Devops Repo') {
            steps {
                 // script {
                //     echo "deploying with ${env.GITHUB_CREDENTIALS}"
                //     echo "build number: ${env.BUILD_NUMBER}"
                //     withCredentials([
                //         usernamePassword(credentialsId: 'github', usernameVariable: 'USER', passwordVariable: 'PASSWORD')
                //     ]) {
                //         echo "some script ${env.USER} ${env.PASSWORD}"
                //     }
                // }
                withCredentials([
                    usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')
                ]) {
                    echo "some script ${env.GIT_USER} ${env.GIT_TOKEN}"
                }
                sh '''
                    echo https://$GIT_TOKEN@github.com/$DEVOP_ORG/$SERVICE_NAME.git
                    git clone https://$GIT_TOKEN@github.com/$DEVOP_ORG/$SERVICE_NAME.git
                '''
                // git push https://<GITHUB_ACCESS_TOKEN>@github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git
                // "https://github.com/${DEVOP_ORG}/${SERVICE_NAME}"
            }
        }
    }
}
