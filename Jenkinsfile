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
                    -Dsonar.login=sqp_a2a2b89a43ebab835cbb4116e2ce075e66e0cf28
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