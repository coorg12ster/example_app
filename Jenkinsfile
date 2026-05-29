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
                    -Dsonar.login=sqp_dc42d843100c136b4945f0ac84fc26b4bdb52e09
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