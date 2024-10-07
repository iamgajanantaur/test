pipeline {
    agent { label 'leader' }

    stages {
        stage ('SCM Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/iamgajanantaur/test.git'
            }
        }

        stage ('Build') {
            steps {
                sh 'docker build -t iamgajanantaur/myflaskapp:latest .'
            }
        }

        stage ('Push docker image to dockerhub') {
            steps {
                sh 'docker push iamgajanantaur/myflaskapp:latest'
            }
        }

        stage ('Remove existing service') {
            steps {
                sh 'docker service rm myflaskappservice'
            }
        }

        stage ('Create a service') {
            steps {
                sh 'docker service create --name myflaskappservice -p 80:4000 --replicas=2 iamgajanantaur/myflaskapp:latest'
            }
        }
    }

    post {
    success {
        script {
            def subject = "Build Successful: ${env.JOB_NAME} - ${env.BUILD_NUMBER}"
            def body = "Good news! The build ${env.BUILD_NUMBER} for job ${env.JOB_NAME} was successful. Check the Jenkins dashboard for more details."

            mail (
                to: 'steve@mailsrv.ditiss.sunbeam',
                subject: subject,
                body: body
            )
        }
    }
    
    failure {
        script {
            def subject = "Build Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}"
            def body = "Unfortunately, the build ${env.BUILD_NUMBER} for job ${env.JOB_NAME} has failed. Please check the Jenkins logs and take the necessary actions."

            mail (
                to: 'steve@mailsrv.ditiss.sunbeam',
                subject: subject,
                body: body
            )
        }
    }
    }
}
