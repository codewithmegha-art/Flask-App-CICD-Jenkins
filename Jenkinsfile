pipeline {
    agent any 

    stages {

        stage('Build') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        } 

        stage('Test') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Deploy') { 
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh 'nohup python3 app.py &'
            }
        } 

        stage('Cleanup') {
            steps {
                sh 'pkill -f app.py || true'
            }
        }
    }

    post {
        success {
            mail to: 'dumbremegha3232@gmail.com',
                 subject: "SUCCESS: Build ${env.BUILD_NUMBER}",
                 body: "Build succeeded!"
        }

        failure {
            mail to: 'dumbremegha3232@gmail.com',
                 subject: "FAILURE: Build ${env.BUILD_NUMBER}",
                 body: "Build failed. Check Jenkins logs."
        }
    }
}
