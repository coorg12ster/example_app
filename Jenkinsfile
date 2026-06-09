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
                    -Dsonar.login=sqp_a6696c56a7ed1d1dcc7e0ab4b2a8444b7ad7c5c7
                    '''
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    def imageName = 'localhost:5000/example_app:0.0.1-SNAPSHOT'
                    sh 'docker build -t ' + imageName + ' .'
                    sh 'docker push ' + imageName
                }
            }
        }
    }
}