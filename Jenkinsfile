pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=spring-app \
                    -Dsonar.host.url=http://sonarqube:9000 \
                    -Dsonar.login=sqp_405f4df9be2a97e6f52ed27050e22aaa999d210a
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t spring-app .'
            }
        }
    }
}