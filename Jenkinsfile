
pipeline {
    agent {
        ecs {
            inheritFrom 'build-example-spot'
        }
    }
    stages {
        stage('Test') {
            steps {
                sh "echo this was executed on a spot instance"
                sh 'sleep 10'
                sh 'echo sleep is done'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
    }
}
