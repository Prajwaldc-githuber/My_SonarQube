pipeline {
    agent any

    tools {
    maven 'M3'     // use the exact name of your Maven installation in Jenkins
    jdk 'JDK21'    // use the exact name of your JDK installation
}

    environment {
        SONARQUBE_TOKEN = credentials('SonarQube')  // Secret Text in Jenkins credentials
        SONARQUBE_URL   = 'http://51.20.130.180:9000'
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

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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
