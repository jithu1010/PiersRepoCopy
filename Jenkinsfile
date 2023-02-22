pipeline {
    agent any
    stages {
        stage('Build Flask App') {
            steps {
                sh '''
                docker build -t stratcastor/flask-lbg-demo:latest -t stratcastor/flask-lbg-demo:build-$BUILD_NUMBER .
                '''
           }
        }
        stage('Build Custom NGINX') {
            steps {
                sh '''
                cd ./nginx
                docker build -t stratcastor/nginx-lbg-demo:latest -t stratcastor/nginx-lbg-demo:build-$BUILD_NUMBER .
                '''
           }
        }
        stage('Push Images') {
            steps {
                sh '''
                docker push stratcastor/flask-lbg-demo:latest
                docker push stratcastor/flask-lbg-demo:build-$BUILD_NUMBER
                docker push stratcastor/nginx-lbg-demo:latest
                docker push stratcastor/nginx-lbg-demo:build-$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                cd ./kubernetes
                kubectl apply -f .
                '''
            }
        }
    }
}
