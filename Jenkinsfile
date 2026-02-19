pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app .'
            }
        }

        stage('Deploy Backend Containers') {
    steps {
        sh '''
        docker network create lab6-net || true
        docker rm -f backend1 backend2 || true

        docker run -d --name backend1 --network lab6-net backend-app
        docker run -d --name backend2 --network lab6-net backend-app
        '''
    }
}

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                docker rm -f nginx-lb || true
                docker run -d --name nginx-lb -p 80:80 nginx
                docker cp nginx-pes2ug24cs817.yaml nginx-lb:/etc/nginx/nginx.conf
                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully. NGINX load balancer is running.'
        }
    }
}
