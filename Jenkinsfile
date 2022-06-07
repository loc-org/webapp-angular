pipeline {
    agent {
        label 'tooling'
    }
    triggers {
        githubPush()
    }
    environment {
        NEW_VERSION = '1.3.0'
        GITHUB_CREDENTIALS = credentials('github')
    }
    stages {
        stage('Build') {
            steps {
                script {
                    echo "deploying with ${env.GITHUB_CREDENTIALS}"
                    echo "build number: ${env.BUILD_NUMBER}"
                    withCredentials([
                        usernamePassword(credentialsId: 'github', usernameVariable: 'USER', passwordVariable: 'PASSWORD')
                    ]) {
                        echo "some script ${env.USER} ${env.PASSWORD}"
                    }
                }
                sh '''
                    docker build -t webapp-angular .
                '''

                
                // sh 'ls'
                // echo 'Login in to Amazon ECR...'
                // // sh ' | docker login --username AWS --password-stdin 398692602192.dkr.ecr.ap-southeast-1.amazonaws.com'
                // sh 'REPO_PWD=$(aws ecr get-login-password --region ap-southeast-1)'
                // sh 'docker login -u AWS -p ${REPO_PWD} 398692602192.dkr.ecr.ap-southeast-1.amazonaws.com'
                // echo 'build image'
                // sh 'docker build -t webapp-angular .'
                // sh 'docker tag webapp-angular:latest 398692602192.dkr.ecr.ap-southeast-1.amazonaws.com/webapp-angular:latest'
                // echo 'push image'
                // sh 'docker push 398692602192.dkr.ecr.ap-southeast-1.amazonaws.com/webapp-angular:latest'
            }
        }
        stage('Test') {
            steps {
                echo 'deploying the application'
            }
        }
        // stage('Clone repository') { 
        //     steps { 
        //         script {
        //             checkout scm
        //         }
        //     }
        // }
        // stage('Build') { 
        //     steps { 
        //         script {
        //             app = docker.build("underwater")
        //         }
        //     }
        // }
        // stage('Test'){
        //     steps {
        //         echo 'Empty'
        //     }
        // }
        // stage('Deploy') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://720766170633.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:aws-credentials') {
        //                 app.push("${env.BUILD_NUMBER}")
        //                 app.push("latest")
        //             }
        //         }
        //     }
        // }
    }
}
