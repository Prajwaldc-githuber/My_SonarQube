pipeline {
    agent any

    tools {
        // Make sure these tools are configured in Jenkins -> Global Tool Configuration
        maven 'Maven3'     // your Maven installation name
        jdk 'JDK21'        // your JDK installation name
    }

    environment {
        SONARQUBE_TOKEN = credentials('sonar-token-id')  // Secret Text in Jenkins credentials
        SONARQUBE_URL   = 'http://localhost:9000'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/jayanthis952/javaparser-maven-sample.git'
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
