pipeline {
  agent any
  tools {
    jdk 'JDK17'          // name must match your Jenkins Global Tool Config
    maven 'MAVEN_3_8_7'  // or 'MAVEN_3_9_6', whichever you configured
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean install'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }
    stage('Run Sample') {
      steps {
        sh 'java -jar target/javaparser-maven-sample-1.0-SNAPSHOT-shaded.jar || true'
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }
}
