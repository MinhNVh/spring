pipeline {

    agent any
    
    environment {
        MYSQL_ROOT_LOGIN = credentials('mysql-root-login')
    }
    stages {
        
        stage('Deploy MySQL to DEV') {
            steps {
                sh 'whoami'
                echo 'Deploying and cleaning'
                sh 'docker image pull mysql:8.0'
                sh 'docker network create dev || echo "this network exists"'
                sh 'docker container stop khalid-mysql || echo "this container does not exist" '
                sh 'echo y | docker container prune '
                sh 'docker volume rm khalid-mysql-data || echo "no volume"'
                sh "docker run --name minh-mysql --rm --network dev -v minh-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_LOGIN_PSW} -e MYSQL_DATABASE=ejbca  -d mysql:8.0 "
                sh 'sleep 20'
                sh "docker exec -i minh-mysql mysql --user=root --password=${MYSQL_ROOT_LOGIN_PSW} < script"
            }
        } 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
