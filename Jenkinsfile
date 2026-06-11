pipeline {
    agent any

    tools {
        maven 'Maven'
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token')
        SONAR_TOKEN = credentials('sonar-token')
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
                    -Dsonar.login=$SONAR_TOKEN
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
        stage('Snyk Container Test') {
            steps {
                sh '''
                docker run --rm --platform linux/amd64 \
                    --entrypoint snyk \
                    -e SNYK_TOKEN=$SNYK_TOKEN \
                    -v /var/run/docker.sock:/var/run/docker.sock \
                    snyk/snyk:maven-3-jdk-21-preview \
                    container test localhost:5000/example_app:0.0.1-SNAPSHOT || true
                '''
            }
        }
        stage('SCA') {
            steps {
                sh 'mvn snyk:test -Dsnyk.token=$SNYK_TOKEN || true'
            }
        }
    }
}