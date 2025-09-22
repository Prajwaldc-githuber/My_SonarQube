pipeline {
    agent any

    tools {
    maven '3.6.3'     // use the exact name of your Maven installation in Jenkins
    jdk '17.0.16'    // use the exact name of your JDK installation
}

    environment {
        SONARQUBE_TOKEN = credentials('SonarQube')  // Secret Text in Jenkins credentials
        SONARQUBE_URL   = 'http://13.53.119.232:9000'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Prajwaldc-githuber/My_SonarQube.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install -B'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {  // 'SonarQube' is the name of SonarQube server in Jenkins configuration
                    sh "mvn sonar:sonar -Dsonar.projectKey=javaparser-maven-sample -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONARQUBE_TOKEN}"
                }
            }
        }

    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs!"
        }
        always {
            cleanWs()
        }
    }
}
